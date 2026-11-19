# Hito 1 — Validación del entorno local

**Proyecto:** ClawOps  
**Hito:** OpenClaw + Telegram + Ollama  
**Fecha de validación:** 19 de noviembre de 2026  
**Estado:** Funcional

## 1. Objetivo

Establecer y validar un entorno local de ejecución de agentes de IA utilizando OpenClaw como Gateway, Telegram como interfaz de comunicación y Ollama como proveedor local de inferencia.

El objetivo es disponer de una base funcional antes de implementar ClawOps Model Router.

## 2. Entorno técnico

| Componente | Versión / configuración |
|---|---|
| Sistema operativo | Windows |
| OpenClaw | 2026.11.5 (`e13cf15`) |
| Ollama | 0.40.16 |
| Node.js | 26.13.1 |
| Docker CLI | 29.8.1 |
| Gateway | `127.0.0.1:18789` |
| Proveedor LLM | Ollama |
| Canal | Telegram |
| Transporte Telegram | Polling |

Nota: se verificó la versión del cliente Docker, no el estado del Docker Engine.

## 3. Arquitectura validada

```text
Telegram
   |
   v
OpenClaw Gateway
   |
   +---- Agent Runtime
   |        |
   |        +---- Tool Execution
   |
   v
Ollama API
   |
   +---- qwen3:8b
   +---- qwen3:1.7b
   +---- qwen2.5-coder:7b
```

OpenClaw conserva la responsabilidad de gestionar sesiones, contexto y ejecución de herramientas. Ollama proporciona la inferencia local.

## 4. Modelos disponibles

| Modelo | Función prevista |
|---|---|
| `qwen3:1.7b` | Modelo pequeño |
| `qwen3:8b` | Modelo predeterminado |
| `qwen2.5-coder:7b` | Tareas orientadas a programación |

Modelo predeterminado validado:

`ollama/qwen3:8b`

La API nativa de Ollama se encuentra configurada en:

`http://127.0.0.1:11434`

API declarada: `ollama`.

Se realizó una prueba directa de tool calling con `qwen3:8b` mediante `/api/chat`. El modelo generó una llamada estructurada a `get_weather`, incluyendo argumentos JSON.

## 5. Validaciones realizadas

### 5.1 OpenClaw Gateway

Comando:

```powershell
openclaw gateway status
```

Resultado:

- Runtime: running.
- Connectivity check: ok.
- Puerto: 18789.
- Bind: loopback (`127.0.0.1`).
- Gateway y CLI: misma versión.

El Gateway permanece accesible únicamente desde el equipo local.

### 5.2 Modelos

Comando:

```powershell
openclaw models status
```

Resultado:

- Modelo predeterminado: `ollama/qwen3:8b`.
- Proveedor: Ollama.
- Fallbacks: ninguno.
- No se requieren credenciales OAuth.

### 5.3 Telegram

Comando:

```powershell
openclaw channels status --probe
```

Resultado:

- Cuenta: `default`.
- Canal: enabled.
- Estado: connected.
- Transporte: polling.
- Bot: `@clawops_lab_bot`.
- Estado de funcionamiento: works.

Se completó el emparejamiento privado del usuario con el bot.

### 5.4 Ejecución de herramientas

Se realizaron pruebas desde Telegram:

- Consulta de hora.
- Solicitud de información del sistema.
- Ejecución de `hostname`.

Resultado obtenido desde Telegram:

`DESKTOP-3B63P14`

Resultado comprobado directamente en PowerShell:

```powershell
hostname
```

`DESKTOP-3B63P14`

Ambos resultados coincidieron. OpenClaw también registró el hito `tool_execution_started` durante la ejecución.

### 5.5 Prueba directa de tool calling en Ollama

Se utilizó el endpoint:

`POST http://127.0.0.1:11434/api/chat`

El modelo `qwen3:8b` generó una llamada estructurada con:

- Nombre: `get_weather`.
- Argumento: `city = Paris`.
- Campo `message.tool_calls` presente.

Esto confirma la capacidad de generación de llamadas estructuradas del modelo a través de la API nativa de Ollama.

## 6. Seguridad y configuración

- Gateway enlazado exclusivamente a `127.0.0.1`.
- Token del Gateway gestionado mediante el secret store de OpenClaw.
- Auditoría de secretos realizada previamente: sin secretos en texto plano en la configuración auditada.
- Telegram utiliza emparejamiento privado.
- No se expone el Gateway a Internet.
- No se incorporan credenciales ni tokens al repositorio.
- No se utilizan servicios Azure de pago en este hito.

## 7. Observaciones conocidas

1. `Service definition inspection incomplete`: el diagnóstico de inspección del servicio no terminó completamente. La conectividad y el runtime sí fueron comprobados.
2. `connected-no-operator-scope`: el estado de conectividad del CLI no dispone del alcance de operador. No impidió la comunicación con Telegram ni la ejecución validada del agente.
3. `DEP_PLUGIN_SDK`: advertencia de deprecación del SDK de plugins. No bloquea el funcionamiento actual.
4. La compatibilidad completa de streaming y tool calling con un futuro Model Router todavía debe validarse.

## 8. Criterios de aceptación

| Criterio | Resultado |
|---|---|
| OpenClaw instalado | Cumplido |
| Gateway local operativo | Cumplido |
| Ollama conectado | Cumplido |
| Inferencia local | Cumplido |
| Telegram conectado | Cumplido |
| Emparejamiento privado | Cumplido |
| Tool calling directo en Ollama | Cumplido |
| Ejecución real de herramienta desde Telegram | Validada |
| Gateway expuesto públicamente | No; permanece local |

## 9. Resultado del hito

El entorno local de ClawOps se encuentra funcional.

Se establece `ollama/qwen3:8b` como modelo predeterminado para continuar con las pruebas.

El siguiente hito consiste en implementar ClawOps Model Router como servicio independiente utilizando Python, FastAPI y Pydantic v2.

El Router deberá integrarse con OpenClaw mediante una interfaz compatible con OpenAI, preservando el comportamiento de herramientas y streaming.

No se incorporarán todavía PostgreSQL, Redis, RAG, MCP, multiagentes ni infraestructura Azure real.

**Estado final: Hito 1 completado.**