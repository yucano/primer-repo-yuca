# Código Arduino Completo - Cuerpo Sabio

## 🔧 CÓDIGO PRINCIPAL ARDUINO NANO V3

```cpp
#include <SoftwareSerial.h>

// Configuración Bluetooth HC-05
SoftwareSerial bluetooth(10, 11); // RX, TX

// Definición de pines para sensores táctiles
const int ZONA_CABEZA = 2;
const int ZONA_GARGANTA = 3;
const int ZONA_CORAZON = 4;
const int ZONA_ESTOMAGO = 5;
const int ZONA_PELVIS = 6;
const int ZONA_PIERNAS = 7;

// LEDs indicadores (opcional)
const int LED_CABEZA = 8;
const int LED_GARGANTA = 9;
const int LED_CORAZON = 12;
const int LED_ESTOMAGO = 13;

// Arrays para manejo eficiente
int pinesSensores[] = {ZONA_CABEZA, ZONA_GARGANTA, ZONA_CORAZON, ZONA_ESTOMAGO, ZONA_PELVIS, ZONA_PIERNAS};
int pinesLEDs[] = {LED_CABEZA, LED_GARGANTA, LED_CORAZON, LED_ESTOMAGO, -1, -1}; // -1 para zonas sin LED
String nombresZonas[] = {"CABEZA", "GARGANTA", "CORAZON", "ESTOMAGO", "PELVIS", "PIERNAS"};
const int numZonas = 6;

// Variables de control
bool estadosAnteriores[6] = {false, false, false, false, false, false};
unsigned long ultimoEnvio[6] = {0, 0, 0, 0, 0, 0};
const unsigned long DEBOUNCE_DELAY = 500; // 500ms entre envíos de la misma zona

// Variables para modo demostración
bool modoDemostracion = false;
unsigned long tiempoUltimDemo = 0;
int zonaActualDemo = 0;

void setup() {
  Serial.begin(9600);
  bluetooth.begin(9600);
  
  // Configurar pines de sensores como INPUT con pull-up interno
  for (int i = 0; i < numZonas; i++) {
    pinMode(pinesSensores[i], INPUT_PULLUP);
  }
  
  // Configurar LEDs como OUTPUT
  for (int i = 0; i < 4; i++) {
    if (pinesLEDs[i] != -1) {
      pinMode(pinesLEDs[i], OUTPUT);
      digitalWrite(pinesLEDs[i], LOW);
    }
  }
  
  // Mensaje de inicio
  Serial.println("=== CUERPO SABIO INICIADO ===");
  bluetooth.println("SISTEMA_LISTO");
  
  // Secuencia de LEDs de bienvenida
  secuenciaBienvenida();
  
  delay(1000);
}

void loop() {
  // Verificar comandos desde la app
  verificarComandosBluetooth();
  
  // Modo demostración automática
  if (modoDemostracion) {
    ejecutarModoDemostracion();
  } else {
    // Modo normal: leer sensores táctiles
    leerSensoresTactiles();
  }
  
  delay(50); // Pequeña pausa para estabilidad
}

void leerSensoresTactiles() {
  for (int i = 0; i < numZonas; i++) {
    bool estadoActual = !digitalRead(pinesSensores[i]); // Invertido por pull-up
    unsigned long tiempoActual = millis();
    
    // Detectar cambio de estado (sensor activado)
    if (estadoActual && !estadosAnteriores[i]) {
      // Verificar debounce
      if (tiempoActual - ultimoEnvio[i] > DEBOUNCE_DELAY) {
        enviarDatosZona(i);
        activarRespuestaVisual(i);
        ultimoEnvio[i] = tiempoActual;
      }
    }
    
    estadosAnteriores[i] = estadoActual;
  }
}

void enviarDatosZona(int indiceZona) {
  // Crear mensaje JSON para la app
  String mensaje = "{";
  mensaje += "\"zona\":\"" + nombresZonas[indiceZona] + "\",";
  mensaje += "\"timestamp\":" + String(millis()) + ",";
  mensaje += "\"intensidad\":" + String(analogRead(A0)) + ","; // Sensor de intensidad opcional
  mensaje += "\"dispositivo\":\"CUERPO_SABIO_V1\"";
  mensaje += "}";
  
  // Enviar por Bluetooth y Serial
  bluetooth.println(mensaje);
  Serial.println("Zona activada: " + nombresZonas[indiceZona]);
  Serial.println("Datos enviados: " + mensaje);
}

void activarRespuestaVisual(int indiceZona) {
  // Encender LED correspondiente
  if (indiceZona < 4 && pinesLEDs[indiceZona] != -1) {
    digitalWrite(pinesLEDs[indiceZona], HIGH);
    delay(200);
    digitalWrite(pinesLEDs[indiceZona], LOW);
  }
  
  // Patrón de parpadeo según la zona
  switch(indiceZona) {
    case 0: // Cabeza - parpadeo rápido
      parpadeoLED(pinesLEDs[0], 3, 100);
      break;
    case 1: // Garganta - parpadeo medio
      parpadeoLED(pinesLEDs[1], 2, 200);
      break;
    case 2: // Corazón - parpadeo lento
      parpadeoLED(pinesLEDs[2], 1, 500);
      break;
    case 3: // Estómago - doble parpadeo
      parpadeoLED(pinesLEDs[3], 2, 150);
      break;
  }
}

void parpadeoLED(int pin, int repeticiones, int duracion) {
  if (pin == -1) return;
  
  for (int i = 0; i < repeticiones; i++) {
    digitalWrite(pin, HIGH);
    delay(duracion);
    digitalWrite(pin, LOW);
    delay(duracion);
  }
}

void verificarComandosBluetooth() {
  if (bluetooth.available()) {
    String comando = bluetooth.readStringUntil('\n');
    comando.trim();
    
    if (comando == "STATUS") {
      bluetooth.println("CUERPO_SABIO_ACTIVO");
    }
    else if (comando == "DEMO_ON") {
      modoDemostracion = true;
      bluetooth.println("MODO_DEMO_ACTIVADO");
    }
    else if (comando == "DEMO_OFF") {
      modoDemostracion = false;
      bluetooth.println("MODO_DEMO_DESACTIVADO");
    }
    else if (comando == "TEST_LEDS") {
      testearLEDs();
    }
    else if (comando.startsWith("ACTIVAR_")) {
      // Comando para activar zona específica desde la app
      String zona = comando.substring(8);
      activarZonaPorComando(zona);
    }
  }
}

void activarZonaPorComando(String zona) {
  for (int i = 0; i < numZonas; i++) {
    if (nombresZonas[i] == zona) {
      enviarDatosZona(i);
      activarRespuestaVisual(i);
      break;
    }
  }
}

void ejecutarModoDemostracion() {
  unsigned long tiempoActual = millis();
  
  if (tiempoActual - tiempoUltimDemo > 3000) { // Cada 3 segundos
    enviarDatosZona(zonaActualDemo);
    activarRespuestaVisual(zonaActualDemo);
    
    zonaActualDemo = (zonaActualDemo + 1) % numZonas;
    tiempoUltimDemo = tiempoActual;
  }
}

void testearLEDs() {
  bluetooth.println("INICIANDO_TEST_LEDS");
  
  for (int i = 0; i < 4; i++) {
    if (pinesLEDs[i] != -1) {
      digitalWrite(pinesLEDs[i], HIGH);
      delay(300);
      digitalWrite(pinesLEDs[i], LOW);
      delay(200);
    }
  }
  
  bluetooth.println("TEST_LEDS_COMPLETADO");
}

void secuenciaBienvenida() {
  // Encender todos los LEDs secuencialmente
  for (int i = 0; i < 4; i++) {
    if (pinesLEDs[i] != -1) {
      digitalWrite(pinesLEDs[i], HIGH);
      delay(200);
    }
  }
  
  delay(500);
  
  // Apagar todos
  for (int i = 0; i < 4; i++) {
    if (pinesLEDs[i] != -1) {
      digitalWrite(pinesLEDs[i], LOW);
    }
  }
}

// Función para monitoreo de estado del sistema
void mostrarEstadoSistema() {
  Serial.println("\n=== ESTADO DEL SISTEMA ===");
  Serial.println("Modo demostración: " + String(modoDemostracion ? "ON" : "OFF"));
  
  for (int i = 0; i < numZonas; i++) {
    bool estado = !digitalRead(pinesSensores[i]);
    Serial.println(nombresZonas[i] + ": " + (estado ? "ACTIVADO" : "INACTIVO"));
  }
  
  Serial.println("========================\n");
}
```

## 🔧 CONFIGURACIÓN AVANZADA DEL HC-05

### Comandos AT para configurar el HC-05:

```cpp
// Código para configurar HC-05 (ejecutar una sola vez)
void configurarHC05() {
  Serial.println("Configurando HC-05...");
  
  bluetooth.print("AT+NAME=CUERPO_SABIO\r\n");
  delay(1000);
  
  bluetooth.print("AT+UART=9600,0,0\r\n");
  delay(1000);
  
  bluetooth.print("AT+PSWD=1234\r\n");
  delay(1000);
  
  Serial.println("Configuración completada");
}
```

## 📡 PROTOCOLO DE COMUNICACIÓN

### Mensajes enviados desde Arduino a la App:

| Mensaje | Significado |
|---------|-------------|
| `SISTEMA_LISTO` | Arduino iniciado correctamente |
| `{"zona":"CABEZA","timestamp":12345,...}` | Sensor activado con datos |
| `CUERPO_SABIO_ACTIVO` | Respuesta a comando STATUS |

### Comandos que puede recibir desde la App:

| Comando | Acción |
|---------|--------|
| `STATUS` | Verificar estado del sistema |
| `DEMO_ON` | Activar modo demostración |
| `DEMO_OFF` | Desactivar modo demostración |
| `TEST_LEDS` | Probar todos los LEDs |
| `ACTIVAR_CABEZA` | Simular activación de zona específica |

## 🛠️ DIAGNÓSTICO Y SOLUCIÓN DE PROBLEMAS

### Código de diagnóstico:

```cpp
void diagnosticoCompleto() {
  Serial.println("\n=== DIAGNÓSTICO DEL SISTEMA ===");
  
  // Test de conectividad Bluetooth
  bluetooth.println("TEST");
  delay(100);
  if (bluetooth.available()) {
    Serial.println("✓ Bluetooth: OK");
  } else {
    Serial.println("✗ Bluetooth: ERROR");
  }
  
  // Test de sensores
  Serial.println("\nTesteo de sensores:");
  for (int i = 0; i < numZonas; i++) {
    int lectura = digitalRead(pinesSensores[i]);
    Serial.println(nombresZonas[i] + " (Pin " + String(pinesSensores[i]) + "): " + String(lectura));
  }
  
  // Test de LEDs
  Serial.println("\nTesteo de LEDs...");
  testearLEDs();
  
  Serial.println("=== FIN DIAGNÓSTICO ===\n");
}
```

## ⚡ OPTIMIZACIONES DE ENERGÍA

### Para uso con batería:

```cpp
#include <avr/sleep.h>
#include <avr/power.h>

void modoAhorroEnergia() {
  // Configurar modo de suspensión
  set_sleep_mode(SLEEP_MODE_IDLE);
  sleep_enable();
  
  // Reducir frecuencia del reloj
  power_adc_disable();
  power_spi_disable();
  power_timer0_disable();
  power_timer2_disable();
  power_twi_disable();
  
  sleep_mode(); // Entrar en suspensión
  
  // Al despertar
  sleep_disable();
  power_all_enable();
}
```

## 🔄 VERSIÓN CON MÚLTIPLES ARDUINOS

Para proyectos más grandes, código maestro que coordina varios Arduinos:

```cpp
// Arduino Maestro - Coordinador
void coordinarMultiplesDispositivos() {
  // Comunicación I2C con otros Arduinos
  Wire.beginTransmission(ARDUINO_CUERPO_1);
  Wire.write("STATUS");
  Wire.endTransmission();
  
  Wire.requestFrom(ARDUINO_CUERPO_1, 32);
  while (Wire.available()) {
    String respuesta = Wire.readString();
    bluetooth.println("DISPOSITIVO_1:" + respuesta);
  }
}
```
