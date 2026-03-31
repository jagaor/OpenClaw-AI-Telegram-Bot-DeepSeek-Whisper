# 🦞 OpenClaw AI Telegram Bot: DeepSeek + Whisper

[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Latest-red.svg)](https://github.com/openclaw/openclaw)

## 📖 Descripción del Proyecto

Este proyecto despliega un asistente de Inteligencia Artificial a través de Telegram alojado en un servidor local (Homelab) utilizando **Docker Compose** y el framework **OpenClaw**. 

Para optimizar el rendimiento y el coste, el bot utiliza una arquitectura de "Cerebro Dual":
* 🧠 **Cerebro Principal (Texto):** Utiliza la API de **DeepSeek** (`deepseek-chat`) por su increíble capacidad de razonamiento y redacción a un coste muy reducido.
* 🎙️ **Oído y Transcripción (Audio):** Utiliza **Whisper de OpenAI** para recibir notas de voz por Telegram, transcribirlas con alta precisión y pasárselas al modelo de lenguaje.

---

## 🚀 Características

* Interacción 100% nativa a través de Telegram (acepta texto y notas de voz).
* Despliegue rápido mediante contenedores de Docker.
* Dashboard web local para la gestión de la pasarela de conexión sin restricciones de origen.
* Costes operativos hiperoptimizados combinando modelos de diferentes proveedores.

---

## 🛠️ Requisitos Previos

1. Tener **Docker** y **Docker Compose** instalados en el host.
2. Un Bot Token de **Telegram** (obtenido a través de *BotFather*).
3. Una API Key de **DeepSeek**.
4. Una API Key de **OpenAI** (para el modelo de Whisper).

---

## 📦 Estructura del Repositorio

Para que el despliegue funcione correctamente, mantén esta estructura de directorios en tu servidor:

```text
openclaw/
├── data/
│   └── openclaw.json
└── docker-compose.yaml
```

---

## ⚙️ Configuración y Despliegue

### 1. Configurar `docker-compose.yaml`
Crea el archivo `docker-compose.yaml` en la raíz del proyecto e inyecta tus claves de API como variables de entorno. Reemplaza los valores entre `< >` por tus datos reales:

```yaml
services:
  openclaw:
    image: ghcr.io/openclaw/openclaw:latest
    container_name: openclaw
    restart: always
    ports:
      - "18789:18789"
    volumes:
      - ./data:/home/node/.openclaw
    environment:
      - DEEPSEEK_API_KEY=<TU_CLAVE_DEEPSEEK>
      - OPENAI_API_KEY=<TU_CLAVE_OPENAI>
      - OPENCLAW_GATEWAY_TOKEN=<TU_PASSWORD_SEGURO_PARA_WEB>
```

### 2. Configurar `data/openclaw.json`
Dentro de la carpeta `data`, crea el archivo `openclaw.json`. Este archivo define los modelos, el token de Telegram y elimina las restricciones de origen para el panel web. Reemplaza los datos entre `< >`:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "deepseek/deepseek-chat"
      }
    }
  },
  "commands": {
    "native": "auto",
    "nativeSkills": "auto",
    "restart": true
  },
  "channels": {
    "telegram": {
      "enabled": true,
      "dmPolicy": "pairing",
      "botToken": "<TU_TOKEN_TELEGRAM>",
      "groupPolicy": "allowlist",
      "streamMode": "partial"
    }
  },
  "gateway": {
    "port": 18789,
    "mode": "local",
    "bind": "lan",
    "auth": {
      "mode": "token",
      "token": "<TU_PASSWORD_SEGURO_PARA_WEB>"
    },
    "controlUi": {
      "allowedOrigins": ["*"]
    }
  },
  "plugins": {
    "entries": {
      "telegram": {
        "enabled": true
      }
    }
  }
}
```

### 3. Permisos y Arranque
Antes de levantar el contenedor, es crucial dar permisos a la carpeta de datos montada para evitar errores de escritura (`EACCES: permission denied`) por parte del usuario interno de Node en Docker. 

Ejecuta estos comandos en tu terminal estando en la carpeta raíz del proyecto:

```bash
# Asignar permisos totales a la carpeta de datos
sudo chmod -R 777 ./data

# Levantar el contenedor en segundo plano
sudo docker compose up -d
```

---

## 🌐 Acceso al Dashboard Web

Una vez que el contenedor muestre el estado `Started`, puedes acceder al panel de control introduciendo la IP local de tu servidor en cualquier navegador web que esté en la misma red:

👉 `http://<IP_LOCAL_DE_TU_SERVIDOR>:18789`

**Nota de acceso:** Utiliza la contraseña definida en la variable `OPENCLAW_GATEWAY_TOKEN` de tu archivo compose para iniciar sesión de forma segura, poder ver los logs en tiempo real y enlazar tus clientes web o móviles.
