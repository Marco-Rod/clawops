# ClawOps — instrucciones para agentes de desarrollo

## Propósito
ClawOps es un laboratorio local-first para aprender y construir una arquitectura de agentes de IA con OpenClaw, Telegram y un servicio independiente de enrutamiento de modelos (ClawOps Model Router). La infraestructura Azure se practica primero mediante emulación local y se valida en Azure real en una etapa posterior.

## Fuente de verdad
Antes de cambiar código o arquitectura, consulta:
1. `README.md`
2. `docs/architecture.md`
3. `docs/technical-decisions.md`
4. `docs/model-routing.md`
5. `docs/api-contracts.md`
6. `docs/security.md`
7. `docs/roadmap.md`
8. `docs/development-workflow.md`

Si existe una contradicción, no inventes una resolución: identifica el conflicto y solicita una decisión. Los documentos aprobados prevalecen sobre suposiciones derivadas del código generado.

## Principios no negociables
- Local-first y sin costos cloud obligatorios durante el desarrollo inicial.
- No introducir servicios de pago, API keys de pago ni recursos Azure reales sin autorización explícita.
- Ollama es el proveedor de inferencia inicial.
- OpenClaw gestiona canal, sesiones y ejecución del agente; ClawOps Model Router es un servicio independiente responsable de seleccionar modelos.
- No acoplar las políticas de routing a nombres concretos de modelos.
- Las decisiones de routing deben ser explicables, observables y testeables.
- No asumir que todos los modelos pueden permanecer cargados simultáneamente.
- No exponer secretos en código, logs, fixtures, documentación ni commits.
- No agregar PostgreSQL, Redis, MCP, RAG, dashboard ni microservicios adicionales al MVP sin una decisión explícita.
- Floci es un emulador para desarrollo; no equivale a Azure real. Toda capacidad relevante debe verificarse contra la versión utilizada.

## Stack acordado
- Python 3.12+
- FastAPI
- Pydantic v2
- Ollama como runtime/proveedor inicial
- OpenClaw como gateway/agente
- Telegram como canal inicial
- Docker y Docker Compose
- Pytest
- Ruff
- Floci AZ para emulación local de servicios Azure
- Bicep para infraestructura Azure futura

SQLAlchemy, Alembic y PostgreSQL quedan como opciones para una etapa posterior, no como dependencias iniciales.

## Flujo obligatorio de trabajo
1. CONTEXT: inspecciona repositorio, documentos y estado actual.
2. PLAN: presenta alcance, archivos afectados, decisiones y riesgos.
3. REVIEW: espera aprobación cuando el cambio altere arquitectura, contratos, seguridad o alcance.
4. BUILD: implementa el cambio más pequeño que satisfaga el objetivo.
5. VALIDATION: ejecuta pruebas, lint y verificaciones pertinentes.
6. CODE REVIEW: revisa diff, regresiones, seguridad, documentación y deuda introducida.
7. REPORT: resume cambios, comandos ejecutados, resultados y pendientes.

No omitas fases para cambios de arquitectura. Para cambios triviales, puedes compactar el proceso, pero debes validar.

## Reglas de implementación
- Prefiere módulos pequeños, responsabilidades explícitas e interfaces tipadas.
- Usa Pydantic para validar entradas/salidas de API.
- Usa configuración mediante variables de entorno y modelos de settings; proporciona `.env.example` sin valores secretos.
- Define timeouts explícitos para llamadas HTTP y al runtime.
- Maneja errores de proveedor, timeout, modelo no disponible y falta de memoria de forma diferenciada.
- No hagas reintentos ilimitados.
- No registres prompts completos, tokens, credenciales ni datos sensibles por defecto.
- Mantén pruebas unitarias para políticas y pruebas de integración para adaptadores.
- No afirmes compatibilidad OpenAI-compatible de OpenClaw hasta validar el contrato real, incluyendo streaming y tool calls.

## Control de alcance
No implementes funcionalidades que no estén en el hito aprobado. No ejecutes despliegues, comandos destructivos, cambios remotos de GitHub ni creación de recursos cloud sin autorización explícita.

## Definition of Done
Un cambio está terminado cuando:
- cumple el criterio de aceptación acordado;
- tiene pruebas relevantes;
- Ruff y las validaciones aplicables pasan;
- no contiene secretos;
- la documentación afectada está actualizada;
- el diff no incluye archivos o cambios ajenos al alcance;
- se informa cualquier limitación que no haya podido verificarse.
