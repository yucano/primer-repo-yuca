# Creación de la App en MIT App Inventor

## 1. Acceder a MIT App Inventor
- Ingresa a MIT App Inventor y accede con tu cuenta de Google

## 2. Crear un nuevo proyecto
- Haz clic en "Create New Project" y nómbralo, por ejemplo, `SientoYComprendo`

## 3. Diseñar la interfaz de usuario

### Componentes necesarios:
- **BluetoothClient:** Para la comunicación con el HC-05
- **ListPicker:** Para seleccionar el dispositivo Bluetooth
- **Label:** Para mostrar mensajes o estados
- **Button:** Para iniciar la conexión y otras acciones
- **Notifier:** Para mostrar notificaciones al usuario

## 4. Configurar los bloques de programación

### Conexión Bluetooth:
- Al hacer clic en el botón de conexión, se abrirá el `ListPicker` con los dispositivos disponibles
- Una vez seleccionado el HC-05, se establecerá la conexión utilizando el `BluetoothClient`

### Recepción de datos:
- Utiliza el evento `When BluetoothClient.AfterReceiving` para manejar los datos recibidos desde el Arduino
- Los datos pueden ser mostrados en un `Label` o utilizados para activar otras funciones en la App

## 🔗 Recursos y ejemplos útiles

Para facilitar tu desarrollo, te recomendamos revisar los siguientes recursos que contienen ejemplos prácticos y archivos `.aia` que puedes importar directamente en MIT App Inventor:

### Tutorial en YouTube sobre HC-05 y MIT App Inventor:
- HC-05 Bluetooth Module with Arduino-MIT App Inventor

### Repositorio en GitHub con ejemplos de Arduino y App Inventor:
- AppInventor_Arduino_Bluetooth

### Comunidad de MIT App Inventor con ejemplos y discusiones:
- Bluetooth HC-06. Arduino. Send. Receive. Send text file. Multitouch. Image

Estos recursos te proporcionarán ejemplos de cómo estructurar tu App y cómo manejar la comunicación entre el Arduino y la App mediante Bluetooth.

## 🛠 Consideraciones adicionales

### Emparejamiento del HC-05:
- Asegúrate de emparejar el módulo HC-05 con tu dispositivo Android antes de intentar conectarte desde la App

### Permisos de la App:
- Verifica que la App tenga los permisos necesarios para utilizar Bluetooth. Esto se puede configurar en las propiedades del componente `BluetoothClient`

### Pruebas y depuración:
- Utiliza el "AI2 Companion" para probar tu App en tiempo real mientras la desarrollas

## 📱 Diseño de la App "Siento y Comprendo"

### 🏠 1. PANTALLA DE INICIO
**Elementos:**
- Logo + nombre: Siento y Comprendo
- Botón: "Iniciar exploración corporal"
- Botón: "Ver historial emocional"
- Botón: "Configuración / Modo niño / adulto"

### 2. PANTALLA DE EXPLORACIÓN CORPORAL
**Interacción con el dispositivo físico:**
Cuando una zona del cuerpo se toca, se activa el sensor → se abre la pantalla específica.

**Opcional sin dispositivo físico:**
Toque digital en silueta interactiva.

### 💓 3. PANTALLAS POR ZONA CORPORAL
Cada zona tiene una pantalla específica con:

- **a) Nombre de la zona** (ej: "Corazón")
- **b) Mensaje emocional según biodecodificación** (ej: "Conflictos de amor, tristeza o desprotección emocional.")
- **c) Sugerencias terapéuticas o pedagógicas**
  - Respiraciones guiadas
  - Dibujar lo que siento
  - Juegos o movimientos corporales
  - Música para la zona (frecuencias, canciones)
- **d) Botón "Registrar lo que siento"** - Formulario sencillo para dejar registro:
  - Emoción actual
  - Parte del cuerpo
  - Actividad realizada
  - Reflexión o dibujo (opcional)
- **e) Botón "Volver al cuerpo"** - Para seguir explorando otras zonas

### 🧠 4. EJEMPLO DE FUNCIONES POR ZONA

| Zona | Emoción Biodecodificada | Actividad Terapéutica |
|------|-------------------------|----------------------|
| Cabeza | Exceso de control, pensamientos repetitivos | Visualización creativa, pintar |
| Garganta | Comunicación bloqueada | Cantar, leer en voz alta, grabarse |
| Corazón | Conflictos afectivos, falta de afecto | Juego con peluches, carta de amor |
| Estómago | Conflictos de poder, digestión de lo vivido | Masaje suave, escuchar sonidos graves |
| Bajo vientre | Miedo, sexualidad, pertenencia | Pintar con el cuerpo, movimiento libre |
| Piernas | Avance, libertad, estabilidad | Caminar con atención, laberintos de piso |

### 📊 5. PANTALLA DE HISTORIAL
- Línea del tiempo emocional
- Zonas exploradas + emociones registradas
- Actividades favoritas
- Descarga en PDF o compartir con terapeuta

### ⚙ 6. CONFIGURACIÓN / MODO
- Selección de edad (niño / adolescente / adulto)
- Idioma
- Notificaciones suaves
- Música de fondo
- Vincular con dispositivo físico (Bluetooth)

### 📱 3. Pantalla inicial de la App (MIT App Inventor)

#### Pantallas (Activities)

| Pantalla | Función |
|----------|---------|
| 🏠 Home | Muestra zonas del cuerpo, recibe señal de Arduino vía Bluetooth |
| 💬 Diagnóstico emocional | Muestra interpretación según zona |
| 🧘 Estrategias sugeridas | Juegos, respiraciones o ideas interactivas |
| ⚙ Configuración Bluetooth | Emparejar HC-05 con el celular |
