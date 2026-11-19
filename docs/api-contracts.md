# Contratos API — ClawOps

## Estado

Los siguientes endpoints son el objetivo de diseño. El contrato final debe validarse con la versión de OpenClaw elegida y con Ollama. No asumir compatibilidad completa antes del spike de integración.

## Endpoints del Router

| Método | Ruta | Propósito |
|---|---|---|
| GET | `/health` | Estado de vida del servicio |
| GET | `/ready` | Dependencias listas |
| GET | `/v1/models` | Modelos/perfiles expuestos |
| POST | `/v1/chat/completions` | Inferencia compatible con OpenAI |
| GET | `/metrics` | Métricas operativas, si se habilitan |

## Chat completions

La entrada debe aceptar el subconjunto necesario del formato OpenAI Chat Completions: `model`, `messages`, `stream`, `temperature`, `max_tokens`/equivalente y parámetros requeridos por OpenClaw. El esquema exacto se fija después de inspeccionar solicitudes reales de OpenClaw.

El campo `model` puede representar un alias/perfil lógico. La política del Router determina el modelo concreto cuando el modo de routing está habilitado.

## Metadatos de routing

Los metadatos internos pueden incluir:
- request ID;
- perfil solicitado;
- perfil seleccionado;
- modelo seleccionado;
- razón de selección;
- fallback utilizado.

No exponer datos internos sensibles ni detalles del host en respuestas públicas.

## Streaming y tool calls

El soporte de streaming y tool/function calls debe probarse como parte del spike de integración. Si el MVP necesita streaming para funcionar correctamente con OpenClaw, implementar y testearlo antes de ampliar funcionalidades.

No ejecutar herramientas dentro del Router en el MVP: el Router selecciona el modelo y transporta la solicitud/respuesta. La ejecución de herramientas permanece bajo el control de OpenClaw.

## Errores

Definir respuestas consistentes para:
- solicitud inválida;
- modelo/perfil desconocido;
- proveedor no disponible;
- timeout;
- falta de recursos;
- error interno.

Los errores deben evitar revelar secretos, rutas locales o información innecesaria del entorno.
