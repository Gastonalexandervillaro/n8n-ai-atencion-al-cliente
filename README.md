# ⚡ Sistema Automatizado de Atención al Cliente con IA (NVIDIA NIM) y Airtable

> 
> **Solución B2B End-to-End**: Este repositorio contiene una arquitectura completa de automatización orientada a optimizar la recepción, clasificación y persistencia de consultas de clientes en tiempo real.



## 📌 Visión General del Proyecto

El sistema intercepta solicitudes entrantes a través de **Webhooks**, analiza el contenido mediante un modelo de lenguaje avanzado (**NVIDIA NIM - Llama 3.1 70B**), determina el nivel de urgencia y genera un **resumen ejecutivo automatizado**. 

Finalmente, almacena la consulta limpia y categorizada en **Airtable** y emite una respuesta JSON inmediata (< 2 segundos).



## 🛠️ Arquitectura y Flujo de Datos


  [ 📥 Cliente / Webhook POST ] 
               ↓
  [ 🧠 Analizar Prioridad y Resumen (LangChain Chain) ]
         ├── 🤖 Modelo: NVIDIA NIM (meta/llama-3.1-70b-instruct)
         └── 📐 Validador: Structured Output Parser (JSON Schema)
               ↓
  [ 💾 Registrar en Base de Datos (Airtable API) ]
               ↓
  [ 📤 Responder al Cliente (HTTP 200 JSON Response) ]

Desglose Técnico de Componentes (Nodos n8n)
📥 Recibir consulta (n8n Webhook Trigger)

Función: Listener HTTP POST encargado de recibir la carga útil (payload) enviada por el cliente (name, email, message).

🧠 Analizar prioridad y resumen (@n8n/n8n-nodes-langchain.chainLLM)

Función: Orquestador principal de IA que evalúa el contenido de la consulta, asigna el nivel de severidad (Alta, Media, Baja) y genera un resumen ejecutivo.

⚙️ NVIDIA NIM (NVIDIA NIM API - meta/llama-3.1-70b-instruct)

Función: Microservicio de inferencia de IA de alta precisión especializado en Procesamiento de Lenguaje Natural (NLP).

📐 Formato del análisis (Structured Output Parser)

Función: Define y valida el esquema JSON de salida (priority, summary) para garantizar la integridad de los datos recibidos.

💾 Registrar en base de datos (Airtable API)

Función: Persistencia de datos. Mapea y sanitiza las variables dinámicas (aplicando formato Capitalize en la prioridad) para guardarlas en la tabla Historial.

📤 Responder al cliente (Webhook Response)

Función: Retorna al cliente una confirmación inmediata en formato JSON con el estado del proceso y los datos asignados.

🔑 Requisitos de Despliegue e Instalación
[!NOTE]
Para replicar este flujo en tu propia instancia de n8n, seguí estos tres pasos:

Importación del Workflow:

Copiar o descargar el archivo workflow.json disponible en este repositorio.

En tu panel de n8n: Workflows ➔ Import from File y seleccionar el JSON.

Configuración de Credenciales:

NVIDIA NIM API Key: Configurar la credencial para acceder a los microservicios de inferencia.

Airtable Personal Access Token: Vincular la base de datos Gestión de Consultas de Clientes y la tabla Historial.

Activación:

Cambiar el interruptor de estado a Active en la esquina superior derecha del editor.

📈 Impacto de Negocio


⏱️ Tiempo de Respuesta: Reducción drástica del tiempo de atención inicial de minutos a < 2 segundos.

🎯 Trazabilidad 100%: Clasificación y registro sin margen de error humano para derivación inmediata a soporte.

⚡ Escalabilidad B2B: Arquitectura desacoplada, orientada a eventos y lista para entornos de producción.
