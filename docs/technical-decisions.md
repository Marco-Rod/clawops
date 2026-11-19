# Decisiones técnicas — ClawOps

Este documento registra decisiones acordadas. Una decisión nueva debe incluir contexto, alternativas, consecuencias y estado.

## DEC-001 — Desarrollo local-first

**Estado:** Acordado.

**Decisión:** El desarrollo y las pruebas iniciales se ejecutan localmente. No se crean recursos Azure reales ni se habilita facturación de pago sin autorización explícita.

**Motivo:** Evitar costos durante el aprendizaje y permitir iteraciones rápidas.

**Consecuencia:** Las pruebas locales no sustituyen la validación final en Azure.

## DEC-002 — OpenClaw como gateway del agente

**Estado:** Acordado; integración por validar.

**Decisión:** OpenClaw administra Telegram, sesiones y ejecución del agente.

**Motivo:** Evitar construir un gateway de agentes propio.

**Consecuencia:** Debe realizarse una prueba técnica del proveedor personalizado/endpoint compatible con OpenAI antes de dar por cerrada la integración.

## DEC-003 — Model Router independiente

**Estado:** Acordado.

**Decisión:** El routing vive en un servicio separado llamado ClawOps Model Router, implementado con Python, FastAPI y Pydantic v2.

**Motivo:** Separar la orquestación del agente de las políticas de selección de modelos y permitir reutilizar el Router desde otras aplicaciones.

## DEC-004 — Ollama como proveedor inicial

**Estado:** Acordado.

**Decisión:** Ollama es el runtime inicial. No se requiere proveedor API de pago.

**Motivo:** Mantener el costo de inferencia en cero y aprender el routing independientemente de un proveedor comercial.

**Consecuencia:** La selección debe respetar memoria, concurrencia, carga/descarga y capacidades reales del hardware.

## DEC-005 — Routing por perfiles, no por nombres

**Estado:** Acordado.

**Decisión:** Las políticas seleccionan perfiles lógicos (`small`, `medium`, `large`). El registro asocia cada perfil a uno o más modelos concretos.

**Motivo:** Cambiar modelos sin reescribir políticas.

## DEC-006 — Routing determinístico antes que routing con LLM

**Estado:** Acordado como estrategia inicial.

**Decisión:** Comenzar con reglas explícitas y observables. Evaluar clasificadores más sofisticados después de establecer una línea base.

**Motivo:** Reducir complejidad, latencia y opacidad.

## DEC-007 — Floci AZ para emulación

**Estado:** Acordado con validación pendiente.

**Decisión:** Utilizar Floci AZ para practicar servicios Azure localmente.

**Motivo:** Aprender interfaces y flujos sin consumir créditos.

**Consecuencia:** Verificar la compatibilidad de cada API y versión. Managed Identity, RBAC, Container Apps, ACR y Key Vault requieren pruebas específicas. No asumir paridad.

## DEC-008 — Infraestructura Azure futura declarada con Bicep

**Estado:** Acordado para la etapa cloud.

**Decisión:** Utilizar Bicep para describir recursos Azure.

**Motivo:** Practicar infraestructura como código nativa de Azure.

## DEC-009 — Persistencia mínima al inicio

**Estado:** Acordado.

**Decisión:** Comenzar con logs estructurados y configuración declarativa. No agregar PostgreSQL/SQLAlchemy/Alembic al primer hito.

**Motivo:** Evitar infraestructura prematura. Incorporar persistencia cuando exista un requisito concreto de historial, métricas o configuración dinámica.
