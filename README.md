# 🦞 OpenClaw AI Telegram Bot: DeepSeek + Whisper

[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Latest-red.svg)](https://github.com/openclaw/openclaw)

## 📖 Descripción del Proyecto

Este proyecto despliega un asistente de Inteligencia Artificial a través de Telegram alojado en un servidor local (Homelab) utilizando **Docker Compose** y el framework **OpenClaw**. 

Para optimizar el rendimiento y el coste, el bot utiliza una arquitectura de "Cerebro Dual":
* 🧠 **Cerebro Principal (Texto):** Utiliza la API de **DeepSeek** (`deepseek-chat`) por su increíble capacidad de razonamiento y redacción a un coste muy reducido.
* 🎙️ **Oído y Transcripción (Audio):** Utiliza **Whisper de OpenAI** (`whisper-1`) para recibir notas de voz por Telegram, transcribirlas con alta precisión y pasárselas al modelo de lenguaje.

## 🚀 Características

* Interacción 100% nativa a través de Telegram (acepta texto y notas de voz).
* Despliegue rápido mediante contenedores de Docker.
* Dashboard web local para la gestión de la pasarela de conexión.
* Costes operativos hiperoptimizados combinando modelos de diferentes proveedores.

## 🛠️ Requisitos Previos

1. Tener **Docker** y **Docker Compose** instalados en el host.
2. Un Bot Token de **Telegram** (obtenido a través de *BotFather*).
3. Una API Key de **DeepSeek**.
4. Una API Key de **OpenAI** (para Whisper).

## 📦 Estructura de Archivos y Despliegue

La estructura del directorio debe ser la siguiente:

```text
openclaw/
├── data/
│   └── openclaw.json
└── docker-compose.yaml
