# 🐳 Bot Generador de Entornos Docker

**DAM · Sistemas Informáticos · Proyecto Integrador — Tercera Evaluación**

Bot de Telegram con IA que, a partir de una descripción en lenguaje natural, genera automáticamente un fichero `docker-compose.yml` personalizado y listo para desplegar.

---

## 📐 Arquitectura del sistema

```
Usuario (Telegram / curl / Postman)
        │
        ▼
   Webhook (n8n)
        │
        ▼
HTTP Request → API Groq (llama-3.3-70b-versatile)
        │
        ▼
   Edit Fields (extrae el compose generado)
        │
        ▼
Respond to Webhook (devuelve el docker-compose.yml)
```

### Servicios implicados

| Servicio | Descripción | Puerto |
|----------|-------------|--------|
| **n8n** | Orquestador del flujo de trabajo | 5678 (expuesto) |
| **OpenHands** | Agente IA para ejecución de tareas | interno (no expuesto) |
| **Groq API** | Modelo LLM gratuito (llama-3.3-70b-versatile) | externo |

Ambos servicios (n8n y OpenHands) corren en una red bridge interna llamada `docker_net`. Solo se expone el puerto de n8n hacia el exterior.

---

## 🚀 Cómo arrancar el proyecto

### Requisitos previos

- [Docker](https://www.docker.com/) y [Docker Compose](https://docs.docker.com/compose/) instalados
- Cuenta gratuita en [Groq](https://console.groq.com/) para obtener una API key

### 1. Clonar el repositorio

```bash
git clone https://github.com/TU_USUARIO/TU_REPOSITORIO.git
cd TU_REPOSITORIO
```

### 2. Levantar los servicios

```bash
docker-compose up -d
```

### 3. Acceder a n8n

Abre el navegador en:

```
http://localhost:5678
```

### 4. Importar el flujo de n8n

1. En n8n, ve a **Settings → Import workflow**
2. Importa el fichero `workflow.json` incluido en este repositorio
3. Configura tu API key de Groq en el nodo **HTTP Request** (header `Authorization: Bearer TU_API_KEY`)

### 5. Probar el bot

Con el flujo activo en n8n, ejecuta desde PowerShell:

```powershell
Invoke-RestMethod -Uri "http://localhost:5678/webhook-test/generar-entorno" -Method POST -ContentType "application/json" -Body '{"mensaje": "quiero un nginx con php"}'
```

O desde Linux/Mac:

```bash
curl -X POST http://localhost:5678/webhook-test/generar-entorno \
  -H "Content-Type: application/json" \
  -d '{"mensaje": "quiero un nginx con php"}'
```

El sistema responderá con un `docker-compose.yml` generado por IA listo para usar.

---

## 📁 Estructura del repositorio

```
📦 proyecto-docker-bot
 ┣ 📄 docker-compose.yml      # Levanta n8n + OpenHands en red bridge
 ┣ 📄 workflow.json            # Exportación del flujo de n8n
 ┣ 📄 README.md                # Este fichero
 ┗ 📄 docs/                    # Capturas de pantalla y documentación
```

---

## 🛠️ Tecnologías utilizadas

- [n8n](https://n8n.io/) — Automatización de flujos
- [Groq](https://groq.com/) — API de LLM gratuita
- [OpenHands](https://github.com/All-Hands-AI/OpenHands) — Agente de IA
- [Docker](https://www.docker.com/) — Contenedores
- [llama-3.3-70b-versatile](https://console.groq.com/docs/models) — Modelo de lenguaje

---

## 👤 Autor

Hugo y Pedro— DAM · Sistemas Informáticos
