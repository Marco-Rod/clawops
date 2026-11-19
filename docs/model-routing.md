# Diseño de ClawOps Model Router

## 1. Objetivo

Seleccionar un modelo adecuado para cada solicitud equilibrando calidad, latencia, capacidades y recursos disponibles. La decisión debe poder explicarse y auditarse.

## 2. Conceptos

- **Task type:** categoría de trabajo, por ejemplo `conversation`, `summarization`, `code_analysis`, `architecture_design`.
- **Complexity:** estimación de complejidad (`low`, `medium`, `high`).
- **Tier:** perfil lógico (`small`, `medium`, `large`).
- **Model:** identificador concreto del proveedor, por ejemplo un modelo disponible en Ollama.
- **Policy:** reglas que relacionan señales de la tarea con un tier.
- **Fallback:** alternativa ante indisponibilidad o fallo.
- **Escalation:** reintento deliberado con un perfil más capaz cuando exista evidencia de insuficiencia y la política lo permita.

## 3. Pipeline de decisión

1. Validar la solicitud.
2. Extraer señales no sensibles: tipo de tarea, longitud/contexto aproximado, capacidades requeridas y límites.
3. Clasificar la tarea mediante reglas determinísticas en el MVP.
4. Determinar el tier recomendado.
5. Consultar el Model Registry.
6. Consultar estado del proveedor y restricciones de recursos.
7. Seleccionar un modelo elegible.
8. Ejecutar la inferencia con timeout.
9. Registrar decisión, latencia, resultado técnico y fallback.
10. Devolver respuesta y metadatos permitidos.

## 4. Perfiles lógicos

Los perfiles son configurables y no representan tamaños físicos obligatorios.

```yaml
profiles:
  small:
    description: "Tareas sencillas y de baja complejidad"
  medium:
    description: "Tareas técnicas moderadas"
  large:
    description: "Tareas complejas que justifican mayor capacidad"
```

La asignación de modelos concretos se realiza en un registro independiente. No fijar nombres de modelos en las reglas de negocio.

## 5. Model Registry

Cada entrada debería describir:
- `provider`
- `model_id`
- `tier`
- `context_window` si se conoce
- `capabilities` (chat, tools, vision, etc.)
- tamaño/cuanti­zación si se conoce
- memoria estimada, indicando que es aproximada
- estado habilitado/deshabilitado
- límites de concurrencia
- timeout recomendado

No inventar capacidades ni memoria. Obtenerlas de configuración validada o de consultas al runtime.

## 6. Resource Manager

El Resource Manager aplica límites conservadores:
- no asumir que varios modelos pueden residir simultáneamente;
- limitar concurrencia;
- verificar disponibilidad del runtime;
- considerar memoria libre y memoria estimada cuando existan métricas fiables;
- permitir descarga/cambio de modelo solamente mediante una política explícita;
- evitar ciclos de carga/descarga que empeoren la latencia.

Si no se puede determinar la memoria disponible de forma fiable, registrar esa limitación y aplicar un límite conservador configurable.

## 7. Fallback y escalamiento

Distinguir:
- error transitorio del proveedor;
- timeout;
- modelo inexistente/no descargado;
- falta de memoria;
- respuesta inválida;
- insuficiencia semántica de la respuesta.

El fallback técnico no implica que la respuesta anterior sea de baja calidad. El escalamiento semántico requiere una señal explícita y una política posterior; no se incluye como comportamiento automático en el primer MVP.

Los reintentos deben ser acotados y no duplicar tool calls con efectos externos.

## 8. Observabilidad

Registrar como mínimo:
- `request_id`
- timestamp
- `task_type`
- `complexity`
- `requested_tier`
- `selected_model`
- `selection_reason`
- `fallback_used`
- `latency_ms`
- resultado técnico
- tokens, solamente si el proveedor los reporta de forma fiable

No registrar prompts completos, secretos ni contenido sensible por defecto.

## 9. Evaluación

Crear un conjunto pequeño de solicitudes representativas y comparar:
- calidad percibida/criterios de evaluación;
- latencia;
- tasa de error;
- selección de tier;
- uso de memoria;
- frecuencia de fallback.

No optimizar únicamente por velocidad ni asumir que un modelo más grande siempre es mejor.
