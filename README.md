# 🎮 Adivina el Número

Una aplicación móvil desarrollada con **Flutter** que implementa un juego interactivo donde el usuario debe adivinar un número secreto entre 1 y 100 en máximo 5 intentos.

## 📱 Características de la Aplicación

### 🎯 Mecánica del Juego
- **Número secreto aleatorio**: Cada partida genera un número aleatorio entre 1 y 100
- **5 intentos disponibles**: El jugador tiene máximo 5 oportunidades para acertar
- **Pistas inteligentes**: La app proporciona retroalimentación indicando si el número es mayor o menor
- **Validación de entrada**: Solo acepta números entre 1 y 100
- **Estados del juego**: Victoria, derrota, y estados intermedios con mensajes dinámicos

### 🏆 Sistema de Puntuación (NUEVO)
- **Almacenamiento persistente**: Usa `SharedPreferences` para guardar el mejor récord
- **Mejor puntuación**: Registra el menor número de intentos necesarios para ganar
- **Visualización en tiempo real**: Muestra el récord actual en la interfaz durante toda la partida
- **Notificación de nuevos récords**: Alerta visual cuando se logra un nuevo personal best
- **Persistencia de datos**: El récord se mantiene incluso después de cerrar la app

### 📋 ListView de Números Intentados (NUEVO)
- **Historial visual**: Muestra los últimos números que el jugador ha intentado
- **Posicionamiento flotante**: Widget ubicado en la esquina superior izquierda de la pantalla
- **Capacidad limitada**: Almacena y muestra hasta los últimos 5 números ingresados
- **Diseño limpio**: Contenedor blanco translúcido con bordes redondeados y sombras
- **Actualización dinámica**: Se actualiza en tiempo real conforme se ingresan números

## 🎨 Interfaz de Usuario

### Componentes principales
1. **Encabezado animado**: Icono giratorio que cambia según el estado del juego
2. **Mensaje principal**: Comunica el estado actual, pistas y resultado
3. **Contador de intentos**: Muestra los intentos restantes con color dinámico (rojo si quedan ≤2)
4. **Récord actual**: Panel que muestra el mejor puntaje logrado
5. **Campo de entrada**: TextField decorado para ingresar números
6. **Botón de acción**: Cambia de estado según el progreso (Adivinar → Felicidades/Perdiste)
7. **Botón de reinicio**: Aparece al finalizar para jugar nuevamente
8. **ListView flotante**: Historial de números intentados en la esquina superior izquierda

### Animaciones
- **Fade-in**: Transición suave al mostrar elementos
- **Slide**: Movimiento desde arriba hacia abajo
- **Rotación**: Icono principal gira continuamente
- **Escalado**: Botón de reinicio se muestra con efecto de zoom

## 🔧 Cambios Realizados al Código

### 1. **Integración de SharedPreferences**
```dart
import 'package:shared_preferences/shared_preferences.dart';
```
Se agregó la dependencia para persistencia de datos.

### 2. **Variables de Control del Sistema de Puntuación**
```dart
late SharedPreferences _prefs;
int _mejorPuntuacion = 999; // Inicializar con un valor alto
```

### 3. **Método de Carga de Datos**
```dart
Future<void> _cargarDatos() async {
  _prefs = await SharedPreferences.getInstance();
  setState(() {
    _mejorPuntuacion = _prefs.getInt('mejorPuntuacion') ?? 999;
  });
  _iniciarJuego();
}
```
Carga el mejor puntaje guardado al iniciar la app.

### 4. **Método de Guardado de Puntuación**
```dart
void _guardarPuntuacion() {
  if (_intentos < _mejorPuntuacion) {
    setState(() {
      _mejorPuntuacion = _intentos;
    });
    _prefs.setInt('mejorPuntuacion', _intentos);
    _mostrarMensajeTemporal('🏆 ¡Nuevo récord! $_intentos intentos', Colors.green);
  }
}
```
Valida y guarda un nuevo récord si es mejor al anterior.

### 5. **Panel de Récord en la UI**
```dart
Container(
  padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 12),
  decoration: BoxDecoration(
    color: Colors.amber.shade50,
    borderRadius: BorderRadius.circular(30),
    border: Border.all(
      color: Colors.amber.shade200,
      width: 2,
    ),
  ),
  child: Row(
    mainAxisSize: MainAxisSize.min,
    children: [
      const Icon(
        Icons.emoji_events,
        color: Colors.amber,
      ),
      const SizedBox(width: 8),
      Text(
        'Récord: ${_mejorPuntuacion == 999 ? '---' : _mejorPuntuacion} intentos',
        style: const TextStyle(
          fontSize: 18,
          fontWeight: FontWeight.bold,
          color: Colors.amber,
        ),
      ),
    ],
  ),
)
```

### 6. **ListView de Números Intentados**
```dart
List<int> _numerosIngresados = [];
```
Lista que almacena los números ingresados durante la partida.

### 7. **Lógica de Actualizacion del Historial**
```dart
_numerosIngresados.add(adivinanza);
if (_numerosIngresados.length > 4) {
  _numerosIngresados.removeAt(0);
}
```
Agrega números nuevos y mantiene solo los últimos 5.

### 8. **Widget Flotante del Historial**
```dart
Positioned(
  left: 10,
  top: 10,
  child: SafeArea(
    child: Container(
      width: 120,
      height: 200,
      decoration: BoxDecoration(
        color: Colors.white.withOpacity(0.95),
        borderRadius: BorderRadius.circular(15),
        boxShadow: [...],
        border: Border.all(
          color: Colors.indigo.shade200,
          width: 2,
        ),
      ),
      child: Column(
        children: [
          // Header
          Container(...),
          // ListView de números
          Expanded(
            child: ListView(...)
          ),
        ],
      ),
    ),
  ),
)
```
Widget posicionado en la esquina superior izquierda que muestra el historial de números.

## 🚀 Cómo Ejecutar

1. Clonar o descargar el proyecto
2. Instalar dependencias:
   ```bash
   flutter pub get
   ```
3. Ejecutar la aplicación:
   ```bash
   flutter run
   ```

## 📦 Dependencias

- **flutter**: Framework principal
- **cupertino_icons**: Iconos del sistema
- **shared_preferences**: ^2.0.0 (para almacemaniento persistente)

## 📄 Estructura del Proyecto

```
lib/
└── main.dart          # Aplicación principal con toda la lógica del juego
```

## 🎓 Aprende Más

- [Flutter Documentation](https://flutter.dev/docs)
- [SharedPreferences Package](https://pub.dev/packages/shared_preferences)
- [Material Design](https://material.io/design)

---

