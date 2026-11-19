# Flujo de desarrollo asistido por agentes

## Objetivo
Mantener cambios pequeños, revisables y reproducibles. El agente de código no debe implementar una solución amplia basándose únicamente en una petición ambigua.

## Fases

### 1. CONTEXT
- Inspeccionar `AGENTS.md` y documentos relevantes.
- Revisar estructura, estado de Git y cambios existentes.
- Identificar comportamiento actual y restricciones.
- No asumir que archivos o funcionalidades existen.

### 2. PLAN
Presentar:
- objetivo;
- alcance incluido y excluido;
- archivos que se crearán/modificarán;
- diseño propuesto;
- pruebas;
- riesgos y preguntas abiertas.

### 3. REVIEW
Esperar aprobación para cambios que alteren:
- arquitectura;
- contratos API;
- seguridad;
- dependencias principales;
- infraestructura;
- alcance del hito.

### 4. BUILD
- Implementar el cambio mínimo.
- Evitar refactors no solicitados.
- Mantener tipos y validaciones.
- Actualizar documentación cuando cambie el comportamiento.

### 5. VALIDATION
Ejecutar las pruebas pertinentes:
- `pytest`
- `ruff check .`
- `ruff format --check .`
- validación de tipos si está configurada;
- pruebas de integración cuando dependencias estén disponibles.

No afirmar que una prueba pasó si no se ejecutó.

### 6. CODE REVIEW
Revisar:
- diff completo;
- regresiones;
- manejo de errores;
- secretos;
- dependencias añadidas;
- compatibilidad con contratos;
- cobertura de pruebas;
- documentación desactualizada.

### 7. REPORT
Entregar resumen, archivos afectados, comandos ejecutados y resultados, limitaciones y próximos pasos sugeridos.

## Reglas de colaboración
- No hacer commits, push, PR ni acciones remotas sin autorización.
- No desplegar infraestructura ni modificar recursos cloud sin autorización.
- No borrar archivos o datos sin aprobación.
- No ampliar el alcance para “aprovechar” una tarea.
- Si una especificación está incompleta, formular la pregunta mínima necesaria.
