# Base de Datos de Biodecodificación

## 🗄️ ESTRUCTURA DE DATOS PARA INTERPRETACIONES EMOCIONALES

### Tabla principal: zonas_corporales

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT | ID único de la zona |
| nombre_zona | VARCHAR(50) | Nombre de la zona corporal |
| descripcion_anatomica | TEXT | Descripción básica de la zona |
| conflicto_emocional | TEXT | Interpretación según biodecodificación |
| mensaje_ninos | TEXT | Explicación adaptada para niños |
| mensaje_adultos | TEXT | Explicación adaptada para adultos |
| color_asociado | VARCHAR(7) | Color hex para representación visual |
| frecuencia_hz | INT | Frecuencia sonora asociada (opcional) |

### Tabla: actividades_terapeuticas

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT | ID único de la actividad |
| zona_id | INT | Referencia a zona corporal |
| nombre_actividad | VARCHAR(100) | Nombre de la actividad |
| tipo_actividad | ENUM | 'respiracion', 'movimiento', 'arte', 'sonido', 'escritura' |
| descripcion | TEXT | Instrucciones de la actividad |
| duracion_minutos | INT | Duración sugerida |
| edad_minima | INT | Edad mínima recomendada |
| edad_maxima | INT | Edad máxima recomendada |
| nivel_dificultad | ENUM | 'facil', 'medio', 'avanzado' |

### Tabla: registros_usuario

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT | ID único del registro |
| usuario_id | VARCHAR(100) | ID anónimo del usuario |
| zona_id | INT | Zona corporal activada |
| fecha_hora | DATETIME | Momento del registro |
| emocion_reportada | VARCHAR(50) | Emoción identificada por el usuario |
| intensidad | INT | Escala 1-10 de intensidad |
| actividad_realizada | INT | ID de actividad realizada |
| notas_usuario | TEXT | Reflexiones o comentarios |
| contexto | VARCHAR(200) | Situación que motivó la consulta |

## 🎯 DATOS ESPECÍFICOS POR ZONA CORPORAL

### 1. CABEZA / CEREBRO
```json
{
  "zona": "Cabeza",
  "conflicto_emocional": "Exceso de control mental, pensamientos obsesivos, autocrítica",
  "mensaje_niños": "Cuando tu cabecita está muy ocupada con pensamientos, necesita descansar y jugar",
  "mensaje_adultos": "Los conflictos mentales pueden manifestarse como tensión, dolores o pensamientos repetitivos",
  "actividades": [
    {
      "nombre": "Dibujo libre de formas",
      "tipo": "arte",
      "descripcion": "Dibuja sin pensar, deja que tu mano se mueva libremente por 5 minutos"
    },
    {
      "nombre": "Respiración 4-7-8",
      "tipo": "respiracion",
      "descripcion": "Inhala 4 segundos, mantén 7, exhala 8. Repite 4 veces"
    }
  ]
}
```

### 2. GARGANTA
```json
{
  "zona": "Garganta",
  "conflicto_emocional": "Dificultades de comunicación, no poder expresar lo que se siente",
  "mensaje_niños": "Tu garganta quiere decir algo importante. ¿Qué necesitas expresar?",
  "mensaje_adultos": "Conflictos relacionados con la expresión, comunicación bloqueada o cosas no dichas",
  "actividades": [
    {
      "nombre": "Tararear melodías",
      "tipo": "sonido",
      "descripcion": "Tararear canciones que te gusten durante 3 minutos"
    },
    {
      "nombre": "Escribir una carta",
      "tipo": "escritura",
      "descripcion": "Escribe una carta a alguien (no es necesario enviarla)"
    }
  ]
}
```

### 3. CORAZÓN / PECHO
```json
{
  "zona": "Corazón",
  "conflicto_emocional": "Conflictos afectivos, rupturas, falta de amor o protección",
  "mensaje_niños": "Tu corazón está sintiendo algo muy fuerte. Vamos a cuidarlo juntos",
  "mensaje_adultos": "Dolores emocionales, pérdidas afectivas, necesidad de protección y cuidado",
  "actividades": [
    {
      "nombre": "Abrazo de peluche",
      "tipo": "movimiento",
      "descripcion": "Abraza fuerte un peluche o almohada por 2 minutos"
    },
    {
      "nombre": "Respiración del corazón",
      "tipo": "respiracion",
      "descripcion": "Pon las manos en el pecho y respira enviando amor a tu corazón"
    }
  ]
}
```

### 4. ESTÓMAGO / ABDOMEN
```json
{
  "zona": "Estómago",
  "conflicto_emocional": "Conflictos de poder, control, digestión de experiencias difíciles",
  "mensaje_niños": "Tu pancita está trabajando para procesar algo que viviste",
  "mensaje_adultos": "Dificultades para 'digerir' situaciones, conflictos de control o injusticia",
  "actividades": [
    {
      "nombre": "Masaje circular suave",
      "tipo": "movimiento",
      "descripcion": "Masajea tu abdomen en círculos lentos, respirando profundo"
    },
    {
      "nombre": "Sonidos graves",
      "tipo": "sonido",
      "descripcion": "Escucha música con sonidos graves o haz sonidos 'OM' profundos"
    }
  ]
}
```

### 5. ZONA PÉLVICA / BAJO VIENTRE
```json
{
  "zona": "Pelvis",
  "conflicto_emocional": "Miedo, inseguridad, temas de identidad y pertenencia",
  "mensaje_niños": "Esta parte de tu cuerpo guarda tu fuerza y seguridad interior",
  "mensaje_adultos": "Conflictos relacionados con la seguridad, la identidad y la estabilidad emocional",
  "actividades": [
    {
      "nombre": "Movimiento de cadera",
      "tipo": "movimiento",
      "descripcion": "Mueve suavemente las caderas en círculos, sentado o de pie"
    },
    {
      "nombre": "Visualización de raíces",
      "tipo": "respiracion",
      "descripcion": "Imagina raíces que salen de tu pelvis hacia la tierra, dándote estabilidad"
    }
  ]
}
```

### 6. PIERNAS / PIES
```json
{
  "zona": "Piernas",
  "conflicto_emocional": "Dificultades para avanzar, miedo al futuro, falta de apoyo",
  "mensaje_niños": "Tus piernas te llevan hacia donde quieres ir. ¿Hacia dónde quieres caminar?",
  "mensaje_adultos": "Conflictos relacionados con el avance en la vida, toma de decisiones, soporte",
  "actividades": [
    {
      "nombre": "Caminar consciente",
      "tipo": "movimiento",
      "descripcion": "Camina muy lento, sintiendo cada paso durante 5 minutos"
    },
    {
      "nombre": "Masaje en pies",
      "tipo": "movimiento",
      "descripcion": "Masajea tus pies mientras piensas en tu camino de vida"
    }
  ]
}
```

## 📊 SISTEMA DE PERSONALIZACIÓN

### Criterios de adaptación de mensajes:

| Edad | Tipo de mensaje | Ejemplo |
|------|----------------|---------|
| 3-7 años | Muy simple, con metáforas | "Tu pancita está conversando contigo" |
| 8-12 años | Explicativo pero amigable | "Tu cuerpo tiene una forma especial de decirte cosas" |
| 13-17 años | Directo pero empático | "Las emociones pueden manifestarse físicamente" |
| 18+ años | Técnico pero accesible | "Según la biodecodificación, esta zona refleja..." |

### Personalización por contexto:

| Contexto | Adaptación |
|----------|------------|
| Escolar | Actividades grupales, tiempo limitado |
| Terapéutico | Más profundidad, seguimiento |
| Familiar | Actividades que incluyan a otros |
| Individual | Reflexión personal, autoconocimiento |

## 🔄 INTEGRACIÓN CON IA

### Prompt base para GPT:
```
Eres un asistente terapéutico especializado en biodecodificación emocional. 
Usuario: [edad], Zona: [zona_corporal], Contexto: [situación]
Responde con empatía, sugiere una actividad apropiada y mantén un tono [infantil/adolescente/adulto].
Máximo 100 palabras.
```

### Ejemplo de respuesta personalizada:
**Input:** "Edad: 8 años, Zona: Corazón, Contexto: Separación de padres"
**Output GPT:** "Tu corazón está sintiendo mucho porque extrañas a alguien especial. Es normal que duela cuando las personas que amamos están lejos. ¿Te animas a hacer un dibujo de tu familia? Mientras dibujas, respira profundo y piensa en todo el amor que tienes dentro."
