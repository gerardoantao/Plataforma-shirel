<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gran Campeón - Simulador de Habilidad</title>
  <style>
    body { background: #0f172a; color: white; font-family: Arial; text-align: center; padding: 20px; margin: 0; }
    .container { max-width: 600px; margin: 50px auto; }
    h1 { font-size: 36px; margin-bottom: 10px; }
    .premio { color: #facc15; font-weight: bold; }
    .contador { background: #1e293b; padding: 20px; border-radius: 10px; margin: 30px 0; border: 1px solid #facc15; }
    .tiempo { font-size: 32px; font-family: monospace; letter-spacing: 2px; }
    button { background: #16a34a; color: white; font-size: 20px; padding: 15px 40px; border: none; border-radius: 8px; cursor: pointer; width: 100%; max-width: 300px; }
    .legal { font-size: 11px; color: #64748b; margin-top: 40px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>¿Sos el #1 en mercados 1-100?</h1>
    <p>Simulador P2P de habilidad. Arrancás con 10.000 fichas gratis.<br>
    Top 1 del mes gana un <span class="premio">iPhone 16</span></p>
    
    <div class="contador">
      <p style="margin:0 0 10px 0; color:#facc15;">TORNEO MENSUAL TERMINA EN:</p>
      <div class="tiempo" id="reloj">23:45:12</div>
    </div>
    
    <button onclick="alert('Pronto: registro gratis')">JUGAR GRATIS AHORA →</button>
    
    <p class="legal">
      +18. Simulador de habilidad con fichas sin valor monetario.<br>
      Bases y condiciones certificadas. Concurso sin obligación de compra.
    </p>
  </div>

  <script>
    const finTorneo = new Date('2026-11-30T23:59:59').getTime();
    setInterval(() => {
      const ahora = new Date().getTime();
      const diff = finTorneo - ahora;
      const hrs = Math.floor(diff / 1000 / 60 / 60);
      const min = Math.floor(diff / 1000 / 60) % 60;
      const seg = Math.floor(diff / 1000) % 60;
      document.getElementById('reloj').innerText = 
        `${String(hrs).padStart(2,'0')}:${String(min).padStart(2,'0')}:${String(seg).padStart(2,'0')}`;
    }, 1000);
  </script>
</body>
</html>
