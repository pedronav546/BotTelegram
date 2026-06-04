# Bot Generador de Entornos Docker

## Arquitectura
[descripción del flujo + captura del diagrama en n8n]

## Cómo arrancar
1. Clona el repositorio
2. Ejecuta `docker compose up -d`
3. Accede a n8n en http://localhost:5678
4. Importa `flujo-n8n.json` desde el menú de n8n

## Prueba rápida
curl -X POST http://localhost:5678/webhook/generar-entorno \
  -H "Content-Type: application/json" \
  -d '{"mensaje": "quiero un entorno Python con PostgreSQL"}'

## Capturas de pantalla
[añade capturas de n8n y de la respuesta del webhook]
