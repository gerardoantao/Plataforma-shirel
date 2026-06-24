import 'package:flutter/material.dart';

void main() {
  runApp(const ShirelEmpleosApp());
}

class ShirelEmpleosApp extends StatelessWidget {
  const ShirelEmpleosApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'SHIREL Empleos',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF0F2027), // Azul oscuro sofisticado
          primary: const Color(0xFF203A43),
          secondary: const Color(0xFFFFB300), // Ámbar para lo Premium
        ),
      ),
      home: const MainNavigationScreen(),
    );
  }
}

class MainNavigationScreen extends StatefulWidget {
  const MainNavigationScreen({super.key});

  @override
  State<MainNavigationScreen> createState() => _MainNavigationScreenState();
}

class _MainNavigationScreenState extends State<MainNavigationScreen> {
  int _selectedIndex = 0;

  // Lista de pantallas para la navegación
  final List<Widget> _screens = [
    const HomeScreen(),
    const Center(child: Text('Mis Suscripciones / Pagos', style: TextStyle(fontSize: 18))),
    const Center(child: Text('Mi Perfil / CV', style: TextStyle(fontSize: 18))),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: _screens[_selectedIndex],
      bottomNavigationBar: NavigationBar(
        selectedIndex: _selectedIndex,
        onDestinationSelected: (int index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        destinations: const [
          NavigationDestination(icon: Icon(Icons.home_outlined), selectedIcon: Icon(Icons.home), label: 'Inicio'),
          NavigationDestination(icon: Icon(Icons.star_border), selectedIcon: Icon(Icons.star), label: 'Premium'),
          NavigationDestination(icon: Icon(Icons.person_outline), selectedIcon: Icon(Icons.person), label: 'Perfil'),
        ],
      ),
    );
  }
}

// PANTALLA PRINCIPAL
class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    // Datos simulados de profesionales
    final List<Map<String, dynamic>> profesionales = [
      {'nombre': 'Carlos Gómez', 'rubro': 'Electricista Matriculado', 'distancia': '0.8 km', 'rating': 4.9, 'premium': true},
      {'nombre': 'Laura Benítez', 'rubro': 'Niñera / Maestra Particular', 'distancia': '1.5 km', 'rating': 4.8, 'premium': true},
      {'nombre': 'Jorge Rodríguez', 'rubro': 'Técnico en Lavarropas', 'distancia': '2.3 km', 'rating': 4.5, 'premium': false},
      {'nombre': 'Marta Martínez', 'rubro': 'Personal de Limpieza (Oficinas)', 'distancia': '3.1 km', 'rating': 4.7, 'premium': false},
    ];

    return Scaffold(
      backgroundColor: const Color(0xFFF8F9FA),
      appBar: AppBar(
        title: const Text(
          'SHIREL empleos',
          style: TextStyle(fontWeight: FontWeight.bold, color: Colors.white, letterSpacing: 1.2),
        ),
        centerTitle: true,
        backgroundColor: const Color(0xFF0F2027),
        elevation: 4,
        actions: [
          IconButton(
            icon: const Icon(Icons.notifications_active, color: Colors.white),
            onPressed: () {},
          ),
        ],
      ),
      body: SingleChildScrollView(
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              // SECCIÓN: BOTÓN DE URGENCIA
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(16),
                decoration: BoxDecoration(
                  gradient: const LinearGradient(
                    colors: [Color(0xFFE65100), Color(0xFFFF8F00)],
                  ),
                  borderRadius: BorderRadius.circular(15),
                  boxShadow: [
                    BoxShadow(color: Colors.orange.withOpacity(0.4), blurRadius: 10, offset: const Offset(0, 4)),
                  ],
                ),
                child: Column(
                  children: [
                    const Text(
                      '¿TENÉS UNA URGENCIA EN TU HOGAR?',
                      style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold, fontSize: 16),
                    ),
                    const SizedBox(height: 5),
                    const Text(
                      'Activá la alerta para conectar al instante con el profesional disponible más cercano.',
                      textAlign: TextAlign.center,
                      style: TextStyle(color: Colors.whiteBF, fontSize: 12),
                    ),
                    const SizedBox(height: 12),
                    ElevatedButton.icon(
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Colors.white,
                        foregroundColor: Colors.redAccent,
                        padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
                        shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(30)),
                      ),
                      icon: const Icon(Icons.flash_on, color: Colors.amber),
                      label: const Text('BOTÓN DE PRIORIDAD / ALERTA', style: TextStyle(fontWeight: FontWeight.bold)),
                      onPressed: () {
                        // Acción del botón de urgencia
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(content: Text('Buscando personal de urgencia en tu zona...')),
                        );
                      },
                    ),
                  ],
                ),
              ),
              const SizedBox(height: 24),

              // SECCIÓN: CATEGORÍAS
              const Text('Categorías Destacadas', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold, color: Color(0xFF203A43))),
              const SizedBox(height: 12),
              SizedBox(
                height: 100,
                child: ListView(
                  scrollDirection: Axis.horizontal,
                  children: [
                    _buildCategoryCard(Icons.electrical_services, 'Electricidad'),
                    _buildCategoryCard(Icons.local_laundry_service, 'Lavarropas'),
                    _buildCategoryCard(Icons.child_care, 'Niñeras'),
                    _buildCategoryCard(Icons.cleaning_services, 'Limpieza'),
                    _buildCategoryCard(Icons.vpn_key, 'Cerrajería'),
                  ],
                ),
              ),
              const SizedBox(height: 24),

              // SECCIÓN: LISTADO (Prioriza Premium y cercanía)
              Row(
                mainAxisAlignment: MainAxisAlignment.between,
                children: [
                  const Text('Profesionales en tu Zona', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold, color: Color(0xFF203A43))),
                  TextButton(onPressed: () {}, child: const Text('Ver todos')),
                ],
              ),
              const SizedBox(height: 8),

              // Generador de la lista de prestadores
              ListView.builder(
                shrinkWrap: true,
                physics: const NeverScrollableScrollPhysics(),
                itemCount: profesionales.length,
                itemBuilder: (context, index) {
                  final prof = profesionales[index];
                  return Card(
                    elevation: prof['premium'] ? 4 : 1,
                    margin: const EdgeInsets.symmetric(vertical: 8),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                      side: prof['premium']
                          ? const BorderSide(color: Color(0xFFFFB300), width: 1.5) // Borde dorado si es premium
                          : BorderSide.none,
                    ),
                    child: ListTile(
                      contentPadding: const EdgeInsets.all(12),
                      leading: CircleAvatar(
                        backgroundColor: const Color(0xFF203A43),
                        child: Text(prof['nombre'][0], style: const TextStyle(color: Colors.white)),
                      ),
                      title: Row(
                        children: [
                          Text(prof['nombre'], style: const TextStyle(fontWeight: FontWeight.bold)),
                          if (prof['premium']) ...[
                            const SizedBox(width: 6),
                            Container(
                              padding: const EdgeInsets.symmetric(horizontal: 6, vertical: 2),
                              decoration: BoxDecoration(color: const Color(0xFFFFB300), borderRadius: BorderRadius.circular(4)),
                              child: const Text('PREMIUM', style: TextStyle(fontSize: 9, fontWeight: FontWeight.bold, color: Colors.black)),
                            ),
                          ]
                        ],
                      ),
                      subtitle: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(prof['rubro'], style: TextStyle(color: Colors.grey[600])),
                          const SizedBox(height: 4),
                          Row(
                            children: [
                              const Icon(Icons.location_on, size: 14, color: Colors.grey),
                              Text(' A ${prof['distancia']}', style: const TextStyle(fontSize: 12)),
                              const SizedBox(width: 12),
                              const Icon(Icons.star, size: 14, color: Colors.amber),
                              Text(' ${prof['rating']}', style: const TextStyle(fontSize: 12, fontWeight: FontWeight.bold)),
                            ],
                          ),
                        ],
                      ),
                      trailing: const Icon(Icons.chevron_right),
                      onTap: () {
                        // Abrir detalle del profesional
                      },
                    ),
                  );
                },
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildCategoryCard(IconData icon, String label) {
    return Container(
      width: 90,
      margin: const EdgeInsets.only(right: 12),
      decoration: BoxDecoration(
        color: Colors.white,
        borderRadius: BorderRadius.circular(12),
        boxShadow: [BoxShadow(color: Colors.black.withOpacity(0.05), blurRadius: 4, offset: const Offset(0, 2))],
      ),
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(icon, size: 32, color: const Color(0xFF203A43)),
          const SizedBox(height: 8),
          Text(label, style: const TextStyle(fontSize: 11, fontWeight: FontWeight.w500), textAlign: Center),
        ],
      ),
    );
  }
}
