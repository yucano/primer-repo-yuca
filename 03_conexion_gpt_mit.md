# Guía de Conexión GPT - MIT App Inventor

## 🛠 Requisitos
1. Una cuenta en OpenAI
2. Una clave API de OpenAI (desde https://platform.openai.com/account/api-keys)
3. Un servidor intermedio (puede ser gratuito usando Replit, Glitch o Render)
4. Tu App en MIT App Inventor

## 🧩 Paso 1: Crear el servidor intermedio (con Replit)

### 1.1 Ir a: https://replit.com
- Crear cuenta
- Crear un nuevo repl con template "Python (Flask)"

### 1.2 Código para el servidor
Pegá este código en `main.py`:

```python
from flask import Flask, request, jsonify
import openai

app = Flask(__name__)
openai.api_key = "TU_CLAVE_API_OPENAI"

@app.route('/chat', methods=['POST'])
def chat():
    data = request.get_json()
    user_input = data.get("mensaje", "")
    
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": user_input}],
        temperature=0.7
    )
    
    respuesta = response.choices[0].message.content.strip()
    return jsonify({"respuesta": respuesta})

app.run(host='0.0.0.0', port=81)
```

**Reemplazá `"TU_CLAVE_API_OPENAI"` por tu clave real.**

### 1.3 Archivo `replit.nix` (si da error de dependencias)
Asegurate de que el archivo contenga:

```nix
{ pkgs }: {
  deps = [
    pkgs.python311
    pkgs.python311Packages.pip
    pkgs.python311Packages.flask
    pkgs.python311Packages.openai
  ];
}
```

### 1.4 Ejecutar el servidor
- Presioná "Run"
- Copiá la URL pública (ejemplo: `https://sientoycomprendo.replit.app`)

## 📱 Paso 2: Conectar desde MIT App Inventor

### 2.1 Componentes necesarios
En la pantalla de tu app:
- 1 `TextBox` (para escribir)
- 1 `Button` (para enviar)
- 1 `Label` (para mostrar la respuesta)
- 1 componente `Web1`

### 2.2 Configurar el Web1
- **URL:** `https://turepl.replit.app/chat`
- **Web1.RequestHeaders:**
  ```
  set Web1.RequestHeaders to create empty list
  ```

### 2.3 Bloques

```
when Button1.Click do
  call Web1.PostText
    url: "https://turepl.replit.app/chat"
    text: join ["{\"mensaje\":\"", TextBox1.Text, "\"}"]

when Web1.GotText do
  set Label1.Text to get responseContent from Web1.GotText
```

Asegurate de que el Web1 esté configurado como `PostText` y tenga el `Content-Type` como JSON.

## 🧪 Paso 3: Probar
- Escribí una frase en el TextBox
- Hacé clic en el botón
- En unos segundos, deberías ver la respuesta de GPT en la etiqueta

## ✅ ¿Qué puede hacer GPT ahora en tu app?
- Interpretar síntomas
- Sugerir juegos, cuentos o actividades
- Escuchar emocionalmente
- Adaptarse al contexto del usuario

## 📲 ¿Cómo se conecta desde la App?

MIT App Inventor no permite conexiones directas seguras a GPT por HTTPS, pero se puede usar un servidor intermedio (Node.js, Python Flask o Google Cloud Functions) que:

1. Reciba el mensaje desde la App
2. Llame a la OpenAI API
3. Devuelva la respuesta a la App
