# Dispositivo Físico "Cuerpo Sabio"

## 🔧 BOCETO TÉCNICO DEL DISPOSITIVO FÍSICO INTERACTIVO

**Nombre tentativo:** "Cuerpo Sabio – Módulo Corporal"

### 🎯 OBJETIVO DEL DISPOSITIVO
Permitir que niños o adultos, al apoyar partes del cuerpo sobre una superficie, puedan:
- Activar sensores que identifican zonas específicas
- Recibir respuestas visuales (luces) o sonoras
- Enviar esa información a la app para obtener una decodificación emocional y actividades asociadas

### 🧱 COMPONENTES PRINCIPALES

#### 1. Plataforma corporal (base de apoyo)
- **Material:** goma eva gruesa, madera liviana, cartón prensado o tela acolchada
- **Tamaño:** adaptable a una persona sentada o acostada (ej.: 90 cm x 60 cm o en partes)
- **Diseño:** silueta del cuerpo humano dividida por zonas (cabeza, pecho, abdomen, piernas, brazos)
- **Ilustraciones:** simples, amigables, tipo "muñeco anatómico escolar"

#### 2. Sensores físicos
- **Tipo:**
  - Botones táctiles simples o
  - Sensores de presión tipo FSR o
  - Cables de cobre (táctiles capacitivos estilo Makey Makey)
- **Ubicación:** colocados debajo o sobre cada zona clave del cuerpo
- **Zonas mínimas sugeridas:**
  - Cabeza
  - Garganta
  - Corazón (pecho)
  - Estómago
  - Bajo vientre
  - Piernas y pies

#### 3. Microcontrolador
- **Recomendado:** Arduino UNO / Nano / ESP32
- **Función:** leer los sensores y enviar señales
- **Salida:**
  - USB para conectar a una computadora o
  - Bluetooth para enviar datos a la app del celular/tablet

#### 4. Salidas multisensoriales (opcionales para enriquecer la experiencia)
- Luces LED por zona (respuesta visual inmediata)
- Parlante o buzzer para sonidos
- Vibración leve (motor vibrador) en respuesta al tacto

### 🔌 ESQUEMA DE CONEXIÓN BÁSICO
```
Sensor de presión/tacto (zona corporal) --> Entrada digital/analógica Arduino
Arduino --> LED/speaker (respuesta local)
Arduino --> App móvil o PC (vía Bluetooth o USB)
```

### 🛠 VERSIÓN CASERA Y EDUCATIVA (BAJO COSTO)
- Usar cartón con zonas pintadas y circuito con papel aluminio
- Arduino + pinzas cocodrilo o placas Makey Makey
- App web sencilla que reconozca la zona activada y muestre información
- Integrar con sonidos grabados por vos o colaboradores

### 🔄 FUNCIONAMIENTO (RESUMEN DE FLUJO)
1. El usuario toca o apoya parte del cuerpo en la plataforma
2. El sensor se activa
3. El microcontrolador identifica la zona
4. La respuesta se muestra:
   - **Localmente:** luz, sonido o vibración
   - **Digitalmente:** la App muestra info emocional + propuesta pedagógica o terapéutica

## 🔌 Diagrama de conexión: Arduino Nano V3 + HC-05 + Zonas táctiles

Este es un esquema básico para 5 zonas del cuerpo (podés escalar hasta 8 o más).

### 🔧 COMPONENTES
- 1x Arduino Nano V3 (ATmega328)
- 1x Módulo Bluetooth HC-05
- 5x Pulsadores o sensores táctiles (uno por zona corporal)
- Resistencias 10kΩ (para "pull-down" en cada botón)
- Cables Dupont o de protoboard
- Protoboard (opcional)

### ⚙ CONEXIONES

| Sensor / Dispositivo | Arduino Nano Pin |
|---------------------|------------------|
| Zona Cabeza | D2 |
| Zona Pecho | D3 |
| Zona Abdomen | D4 |
| Zona Piernas | D5 |
| Zona Brazos | D6 |
| HC-05 TX | D10 (Nano RX simulado) |
| HC-05 RX | D11 (Nano TX simulado) |
| HC-05 VCC | +5V |
| HC-05 GND | GND |

⚠ **Importante:** Usamos la librería `SoftwareSerial` para comunicar el Nano con el HC-05 por los pines D10 y D11.

### 🧠 Código base Arduino

```cpp
#include <SoftwareSerial.h>

SoftwareSerial BT(10, 11); // RX, TX para HC-05

int zonas[] = {2, 3, 4, 5, 6}; // Pines de entrada
String nombresZonas[] = {"Cabeza", "Pecho", "Abdomen", "Piernas", "Brazos"};
int numZonas = 5;

void setup() {
  Serial.begin(9600);
  BT.begin(9600); // velocidad HC-05
  
  for (int i = 0; i < numZonas; i++) {
    pinMode(zonas[i], INPUT);
  }
}

void loop() {
  for (int i = 0; i < numZonas; i++) {
    if (digitalRead(zonas[i]) == HIGH) {
      BT.println(nombresZonas[i]); // Enviar a la App
      delay(500); // evitar múltiples envíos
    }
  }
}
```

## ✅ VENTAJAS DEL ARDUINO NANO V3 (ATmega328):

| Característica | Descripción |
|----------------|------------|
| 💰 Económico | Más barato que un Arduino Uno o Mega |
| 📏 Compacto | Tamaño ideal para meter dentro de una maqueta o muñeco |
| 🔌 Fácil de programar | Compatible con el IDE de Arduino |
| 🔧 Suficiente para sensores | Puede manejar entradas digitales y analógicas |
| 🔄 USB incluido | Ya trae cable para cargar código desde la PC |

### 🧠 APLICACIÓN EN TU PROYECTO "CUERPO SABIO":

1. **Sensores táctiles o de presión por zona corporal**
   Podés conectar hasta 6–8 sensores fácilmente (como botones, touchpads o sensores capacitivos).

2. **Identificación de la zona tocada**
   Cada zona del cuerpo está conectada a un pin digital del Nano. Al tocarla → se activa la entrada → se envía señal.

3. **Comunicación con App (vía Bluetooth o USB)**
   Para que interactúe con la App, necesitás agregar un módulo Bluetooth barato:
   - 🔷 HC-05 o HC-06 Bluetooth: muy económicos y fáciles de usar
   - 📱 La App "escucha" la señal enviada desde el Nano y muestra la pantalla correspondiente

### ⚠ LIMITACIONES A TENER EN CUENTA:

| Limitación | Solución |
|------------|----------|
| No tiene Bluetooth integrado | Añadir módulo HC-05 / HC-06 |
| Pocos pines comparado con Mega | Es suficiente si usás hasta 8 zonas corporales |
| No tiene entrada para baterías directa | Podés alimentarlo con Power Bank vía USB |

### 🔌 ESQUEMA BÁSICO DE CONEXIÓN (ejemplo):
```
[Zona Cabeza] — Pin D2
[Zona Pecho] — Pin D3
[Zona Vientre] — Pin D4
[Zona Piernas] — Pin D5
Bluetooth TX/RX — D6 / D7
Alimentación — Vía USB o VIN + GND
```

### 🔩 Materiales sugeridos para empezar:

| Componente | Aprox. Precio (ARG) |
|------------|-------------------|
| Arduino Nano V3 | $8.000–5.000 |
| Módulo HC-05/06 Bluetooth | $6.000 |
| 6–8 sensores/botones | $2.000 total |
| Cables + protoboard | $8.000 |
| App hecha en MIT App Inventor | Gratuita |
