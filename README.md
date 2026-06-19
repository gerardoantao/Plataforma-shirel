import React, { useState, useEffect, useRef } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getFirestore, 
  doc, 
  setDoc, 
  getDoc, 
  onSnapshot,
  updateDoc,
  collection,
  addDoc
} from 'firebase/firestore';
import { 
  getAuth, 
  signInWithCustomToken, 
  signInAnonymously, 
  onAuthStateChanged 
} from 'firebase/auth';

// --- CONFIGURACIÓN DE AUDIO SINTETIZADO (Web Audio API) ---
class SoundManager {
  constructor() {
    this.ctx = null;
  }
  init() {
    if (!this.ctx) {
      this.ctx = new (window.AudioContext || window.webkitAudioContext)();
    }
  }
  playCoin() {
    this.init();
    if (!this.ctx) return;
    const now = this.ctx.currentTime;
    const osc = this.ctx.createOscillator();
    const gain = this.ctx.createGain();
    osc.type = 'sine';
    osc.frequency.setValueAtTime(587.33, now); // D5
    osc.frequency.setValueAtTime(880, now + 0.1); // A5
    osc.frequency.setValueAtTime(1174.66, now + 0.2); // D6
    gain.gain.setValueAtTime(0.12, now);
    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
    osc.connect(gain);
    gain.connect(this.ctx.destination);
    osc.start(now);
    osc.stop(now + 0.4);
  }
  playTick() {
    this.init();
    if (!this.ctx) return;
    const now = this.ctx.currentTime;
    const osc = this.ctx.createOscillator();
    const gain = this.ctx.createGain();
    osc.type = 'triangle';
    osc.frequency.setValueAtTime(180, now);
    osc.frequency.exponentialRampToValueAtTime(900, now + 0.05);
    gain.gain.setValueAtTime(0.1, now);
    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.06);
    osc.connect(gain);
    gain.connect(this.ctx.destination);
    osc.start(now);
    osc.stop(now + 0.06);
  }
  playWin() {
    this.init();
    if (!this.ctx) return;
    const now = this.ctx.currentTime;
    const notes = [523.25, 659.25, 783.99, 1046.50, 1318.51]; // C5, E5, G5, C6, E6
    notes.forEach((freq, idx) => {
      const osc = this.ctx.createOscillator();
      const gain = this.ctx.createGain();
      osc.type = 'sawtooth';
      osc.frequency.setValueAtTime(freq, now + idx * 0.08);
      gain.gain.setValueAtTime(0.08, now + idx * 0.08);
      gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.08 + 0.4);
      osc.connect(gain);
      gain.connect(this.ctx.destination);
      osc.start(now + idx * 0.08);
      osc.stop(now + idx * 0.08 + 0.4);
    });
  }
  playLose() {
    this.init();
    if (!this.ctx) return;
    const now = this.ctx.currentTime;
    const osc = this.ctx.createOscillator();
    const gain = this.ctx.createGain();
    osc.type = 'sawtooth';
    osc.frequency.setValueAtTime(280, now);
    osc.frequency.linearRampToValueAtTime(80, now + 0.5);
    gain.gain.setValueAtTime(0.15, now);
    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.5);
    osc.connect(gain);
    gain.connect(this.ctx.destination);
    osc.start(now);
    osc.stop(now + 0.5);
  }
  playMatch() {
    this.init();
    if (!this.ctx) return;
    const now = this.ctx.currentTime;
    const osc = this.ctx.createOscillator();
    const gain = this.ctx.createGain();
    osc.type = 'triangle';
    osc.frequency.setValueAtTime(440, now);
    osc.frequency.linearRampToValueAtTime(880, now + 0.35);
    gain.gain.setValueAtTime(0.12, now);
    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.35);
    osc.connect(gain);
    gain.connect(this.ctx.destination);
    osc.start(now);
    osc.stop(now + 0.35);
  }
}

const sound = new SoundManager();

// --- CONFIGURACIÓN DE FIRESTORE (REGLA 1) ---
const appId = typeof __app_id !== 'undefined' ? __app_id : 'shirel-juego-vida-v2';
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

const BOT_OPONENTS = [
  { name: "Gonzalo_VIP", avatar: "👑", badge: "High Roller", winRate: "65%" },
  { name: "Micaela_Shirel", avatar: "💎", badge: "Leyenda", winRate: "59%" },
  { name: "Nico_Efectivo", avatar: "💵", badge: "Tiburón", winRate: "54%" },
  { name: "Sofi_Suerte", avatar: "🍀", badge: "Suertuda", winRate: "58%" },
  { name: "Rey_Del_Tablero", avatar: "🏆", badge: "Invicto", winRate: "70%" },
  { name: "Plata_O_Miedo", avatar: "🔥", badge: "Súper VIP", winRate: "72%" },
];

const GAME_TYPES = [
  { name: "Dados de Oro", icon: "🎲", desc: "Suma de dos dados más alta gana el pozo." },
  { name: "Carta Imperial", icon: "🃏", desc: "Elige una carta, la de mayor rango se lo lleva." },
  { name: "Ruleta Shirel", icon: "🎡", desc: "Gira la ruleta de color. ¡Acierta y gana!" },
  { name: "Moneda de la Suerte", icon: "🪙", desc: "Cara o ceca clásico, velocidad máxima." }
];

export default function App() {
  // --- ESTADOS DE USUARIO & AUTENTICACIÓN ---
  const [user, setUser] = useState(null);
  const [balance, setBalance] = useState(0);
  const [globalJackpot, setGlobalJackpot] = useState(1000000); // El gran millón de Shirel
  const [trafficFactor, setTrafficFactor] = useState(0.8);
  const [cells, setCells] = useState([]);
  const [selectedCell, setSelectedCell] = useState(null);

  // --- ESTADO DE CAJA SHIREL (OPERADOR) ---
  const [shirelCommissionBox, setShirelCommissionBox] = useState(0);
  const [adminMode, setAdminMode] = useState(false);

  // --- MODALES FINANCIEROS REALES ---
  const [showDepositModal, setShowDepositModal] = useState(false);
  const [showWithdrawModal, setShowWithdrawModal] = useState(false);
  const [depositAmount, setDepositAmount] = useState(25000);
  const [depositMethod, setDepositMethod] = useState('MercadoPago');
  const [withdrawAmount, setWithdrawAmount] = useState(10000);
  const [withdrawAlias, setWithdrawAlias] = useState('');
  const [adminWithdrawAmount, setAdminWithdrawAmount] = useState(0);
  const [adminWithdrawCBU, setAdminWithdrawCBU] = useState('');

  // Modales generales y rueda diaria
  const [showDailyWheel, setShowDailyWheel] = useState(false);
  const [dailySpinUsed, setDailySpinUsed] = useState(false);
  const [customAlert, setCustomAlert] = useState(null);
  const [liveFeed, setLiveFeed] = useState([
    { id: 1, text: "🔥 ¡La mesa del Casillero 77 está al rojo vivo! Comisión rebajada al 5.1%" },
    { id: 2, text: "💰 Gonzalo_VIP acaba de ganar $420,000 en el Casillero 99!" }
  ]);

  // --- ESTADOS DE LA PARTIDA ---
  const [gameModalOpen, setGameModalOpen] = useState(false);
  const [betAmount, setBetAmount] = useState(5000);
  const [gameState, setGameState] = useState('lobby'); // 'lobby', 'matching', 'playing', 'result'
  const [opponent, setOpponent] = useState(null);
  const [matchProgress, setMatchProgress] = useState(0);
  const [miniGameData, setMiniGameData] = useState({});
  const [commissionRate, setCommissionRate] = useState(7.5);
  const [gameSelection, setGameSelection] = useState(GAME_TYPES[0]);

  // --- COMPORTAMIENTO FANTASÍA: CONFETI/DESTELLOS AL GANAR ---
  const [triggerConfetti, setTriggerConfetti] = useState(false);

  // --- EFECTO 1: AUTENTICACIÓN ANÓNIMA O CON TOKEN (REGLA 3) ---
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("Error al autenticar:", err);
      }
    };
    initAuth();

    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
    });
    return () => unsubscribe();
  }, []);

  // --- EFECTO 2: SINCRONIZACIÓN FIRESTORE (REGLA 1 Y 2) ---
  useEffect(() => {
    if (!user) return;

    // Sincronizar Balance del Jugador (Ruta Privada - Regla 1)
    const userDocRef = doc(db, 'artifacts', appId, 'users', user.uid, 'profile', 'financials');
    const unsubUser = onSnapshot(userDocRef, (docSnap) => {
      if (docSnap.exists()) {
        const data = docSnap.data();
        setBalance(data
