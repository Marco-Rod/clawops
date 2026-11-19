# Seguridad — ClawOps

## Principios
- Secure by default.
- Mínimos privilegios.
- Secretos fuera del repositorio.
- Herramientas con permisos explícitos.
- Separación entre entorno local y cloud.

## Secretos
- No versionar tokens de Telegram, claves API, contraseñas ni credenciales.
- Mantener `.env` fuera de Git y proporcionar `.env.example` con nombres y valores ficticios.
- Evitar secretos en logs, trazas, capturas y documentación.
- En Azure futuro, almacenar secretos en Key Vault y acceder mediante identidad administrada y RBAC cuando la integración esté validada.

## Telegram y acceso al agente
- Restringir el acceso del bot mediante el mecanismo de allowlist/emparejamiento que soporte la configuración seleccionada.
- No habilitar comandos administrativos para usuarios no autorizados.
- No exponer el Gateway directamente a Internet sin un diseño de autenticación y red revisado.
- Revisar permisos de grupos y comportamiento ante mensajes de usuarios desconocidos.

## Herramientas y ejecución
- Empezar con herramientas de solo lectura.
- No permitir ejecución arbitraria de shell, cambios de archivos o acciones externas sin autorización y controles.
- Separar herramientas del agente de la lógica de routing.
- Aplicar límites de tiempo y tamaño a entradas y salidas.

## Logs y privacidad
- No registrar prompts completos por defecto.
- Redactar tokens y cabeceras de autorización.
- Usar IDs de correlación no sensibles.
- Definir retención limitada para logs locales.

## Docker y Floci
- No publicar puertos innecesarios en la red local.
- No montar el socket Docker en OpenClaw salvo necesidad justificada y aprobada.
- Tratar Floci como un entorno de pruebas, no como un límite de seguridad.
- No usar datos o credenciales reales en fixtures.

## Azure futuro
- Separar identidades para pull de ACR y acceso a Key Vault cuando sea viable.
- Asignar únicamente roles necesarios.
- No activar Pay-As-You-Go ni crear recursos reales sin autorización explícita.
- Definir alertas y presupuestos antes de desplegar recursos facturables.
