# ClawOps

**ClawOps** es un laboratorio local-first para aprender arquitectura de agentes, enrutamiento inteligente de modelos y despliegue cloud.

## Objetivo

Construir un asistente accesible desde Telegram mediante OpenClaw y un servicio independiente, **ClawOps Model Router**, que seleccione el modelo de IA adecuado para cada solicitud según su tipo, complejidad, capacidades requeridas y recursos disponibles.

El foco principal no es encontrar el modelo más grande, sino comprender y medir las decisiones de routing.

## Arquitectura resumida

```text
Telegram
   |
   v
OpenClaw Gateway
   |
   v
ClawOps Model Router (FastAPI)
   |-- Task classification
   |-- Routing policy
   |-- Model registry
   |-- Fallback / escalation
   |-- Resource awareness
   |-- Observability
   |
   v
Ollama
   |-- Modelos locales registrados
```

Docker Compose proporciona el entorno local. Floci AZ se utiliza para practicar integraciones con servicios Azure emulados. Azure real se reserva para una validación posterior y autorizada.

## Stack acordado

- Python 3.12+
- FastAPI y Pydantic v2
- Ollama
- OpenClaw
- Telegram
- Docker Compose
- Pytest y Ruff
- Floci AZ
- Bicep para la futura infraestructura Azure

## Principios

- Local-first: no se requieren recursos cloud de pago para el MVP.
- Ollama es el proveedor inicial.
- OpenClaw y el Model Router tienen responsabilidades separadas.
- El routing debe ser explicable, medible y reproducible.
- Los perfiles `small`, `medium` y `large` no deben depender de nombres concretos de modelos.
- La disponibilidad de memoria y la concurrencia forman parte de la decisión.
- Emular Azure no demuestra compatibilidad completa con Azure real.

## Estado

Este paquete define la arquitectura y las reglas iniciales. No representa una implementación funcional ni confirma que las integraciones ya estén probadas.

Consulta `docs/roadmap.md` para el orden de implementación y `docs/development-workflow.md` para el proceso de trabajo.
