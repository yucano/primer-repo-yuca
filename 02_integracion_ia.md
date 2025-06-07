# Integración de Inteligencia Artificial

## 🧠 ¿Qué puede hacer la IA en Siento y Comprendo?

### 1. Análisis Inteligente de Síntomas
- **Entrada:** Datos del usuario (zona corporal tocada, síntomas registrados, emociones percibidas)
- **IA:** Utiliza modelos de clasificación o inferencia entrenados para:
  - Sugerir posibles conflictos emocionales según la biodecodificación
  - Distinguir patrones comunes entre usuarios
  - Evolucionar con los datos y personalizar respuestas

### 2. Sugerencias Dinámicas y Personalizadas
- La IA aprende del comportamiento del usuario:
  - ¿Qué actividades prefiere (juegos, meditación, lectura)?
  - ¿Qué estrategias le ayudan más?
- Propone actividades pedagógicas adaptadas (estilo Duolingo o Calm)

### 3. Chatbot Terapéutico y Pedagógico
- Integración de un asistente conversacional tipo ChatGPT:
  - Ayuda al usuario a poner en palabras lo que siente
  - Sugiere actividades, respiraciones, o juegos específicos
  - Puede conversar con un lenguaje infantil o adulto

### 4. Entrenamiento local o en la nube
- Se puede entrenar un modelo simple en local (TinyML) o usar servidores externos (Firebase + IA externa)

## 🔧 Tecnologías Recomendadas

| Componente | Tecnología recomendada |
|------------|------------------------|
| Motor de IA | GPT (OpenAI API), TensorFlow Lite, DialogFlow |
| App | MIT App Inventor con conexión a servidor externo (Node-RED o Firebase) |
| IA embebida | TinyML (si se quiere mantener offline) |
| Datos del usuario | Guardar en Firebase o base de datos local para análisis futuro |
| Chatbot | API de OpenAI, Dialogflow o Rasa con adaptaciones pedagógicas |

## 🎯 Caso de uso concreto

**Ejemplo:**
1. Niño/a toca "pecho" en el sensor
2. App detecta ansiedad → IA sugiere: "¿Estás nervioso por algo? Tal vez un juego te ayude a relajarte"
3. IA adapta el juego según edad, historial y hora del día (ej. relajación antes de dormir)

## 🔌 Conexión con OpenAI API

### ¿Qué permite esta conexión con GPT (OpenAI API)?

1. **Asistente conversacional emocional/pedagógico**
   - El usuario escribe o habla (voz a texto)
   - GPT responde de forma empática, adaptada a su edad y contexto

2. **Interpretación contextual de síntomas**
   - Envías datos como: "Síntoma: opresión en el pecho. Usuario: 8 años. Estado: triste."
   - GPT devuelve: "Posible emoción: miedo o tristeza por separación. Actividad sugerida: juego con respiración profunda y abrazo."

3. **Propuestas de actividades personalizadas**
   - GPT sugiere juegos, cuentos, dinámicas, meditaciones o frases según el estado emocional detectado

4. **Traducción a lenguaje comprensible para el usuario**
   - GPT puede traducir términos técnicos a lenguaje infantil o coloquial automáticamente

### Ejemplo del flujo

**Entrada:**
```json
{
  "zona_tocada": "abdomen",
  "síntoma": "dolor punzante",
  "edad": 10,
  "estado_emocional": "nervioso",
  "contexto": "separación reciente de sus padres"
}
```

**Respuesta de GPT:**
"Parece que tu pancita está hablándote porque estás atravesando un momento difícil. Tal vez un dibujo o una historia pueda ayudarte. ¿Querés que te cuente una sobre un niño valiente que pasó por algo parecido?"

### Clave API de ejemplo
```
AIzaSyAcbdG34NkQI6CFLMMnf4PQPYar1pKApRI
```
