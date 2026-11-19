# Roadmap de ClawOps

## Hito 0 — Preparación y especificación
- [x] Definir objetivo local-first.
- [x] Acordar OpenClaw + Telegram.
- [x] Acordar Router independiente con FastAPI.
- [x] Acordar Ollama como proveedor inicial.
- [x] Definir Floci AZ como emulador a evaluar.
- [ ] Verificar versiones, licencias y capacidades exactas de Floci.
- [ ] Crear repositorio/estructura de trabajo y revisar documentos con el usuario.

## Hito 1 — Spike OpenClaw + Telegram + Ollama
- Instalar OpenClaw localmente.
- Crear bot de Telegram.
- Conectar un modelo Ollama.
- Validar conversación, sesiones y controles de acceso.
- Documentar configuración sin secretos.

**Aceptación:** conversación estable desde Telegram usando inferencia local.

## Hito 2 — Router mínimo
- Crear servicio FastAPI.
- Implementar health/readiness.
- Implementar adaptador Ollama.
- Definir contrato mínimo de chat.
- Añadir pruebas unitarias y de integración.

**Aceptación:** una solicitud llega al Router y obtiene respuesta de Ollama.

## Hito 3 — Integración OpenClaw → Router
- Inspeccionar el formato real de solicitudes de OpenClaw.
- Validar endpoint personalizado/OpenAI-compatible.
- Probar streaming y tool calls.
- Ajustar contrato y documentación.

**Aceptación:** OpenClaw utiliza el Router como proveedor sin perder las capacidades necesarias para el MVP.

## Hito 4 — Routing por perfiles
- Model Registry.
- Perfiles small/medium/large.
- Política determinística.
- Razón de selección.
- Pruebas con casos representativos.

**Aceptación:** solicitudes de prueba se enrutan según reglas reproducibles.

## Hito 5 — Resiliencia y recursos
- Timeouts y errores diferenciados.
- Fallback técnico acotado.
- Límites de concurrencia.
- Gestión conservadora de memoria/carga de modelos.
- Logs estructurados.

**Aceptación:** el sistema falla de forma controlada cuando un modelo no está disponible o faltan recursos.

## Hito 6 — Emulación Azure con Floci
- Levantar Floci AZ.
- Probar Key Vault, ACR y Container Apps de forma individual.
- Registrar APIs soportadas y limitaciones.
- Probar publicación/descarga de imagen si está soportada.

**Aceptación:** pruebas reproducibles documentadas para cada servicio; no se presume paridad total.

## Hito 7 — Preparación cloud con Bicep
- Diseñar módulos ACR, Key Vault, Container Apps y almacenamiento.
- Revisar identidades, RBAC, red y persistencia.
- Estimar costos y establecer límites.

**Aceptación:** plan de despliegue revisado sin crear recursos.

## Hito 8 — Validación Azure real (opcional y autorizada)
- Confirmar crédito disponible y vigencia.
- Revisar presupuesto y protección de gasto.
- Obtener aprobación explícita.
- Desplegar el mínimo necesario.
- Validar identidad, secretos, persistencia y logs.
- Eliminar recursos al finalizar si corresponde.

**Aceptación:** despliegue reproducible y costos revisados.

## Fuera de alcance hasta nueva decisión
- RAG.
- MCP.
- Multi-agent orchestration.
- Dashboard.
- PostgreSQL y analítica persistente.
- Proveedores LLM de pago.
- Ejecución autónoma de acciones con efectos externos.
