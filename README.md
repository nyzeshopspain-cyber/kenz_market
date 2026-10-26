# KENZ MARKET BOT — guía de instalación

Bot para Discord con Node.js 20+ y discord.js v14. Incluye moderación, anti-menciones masivas, filtro de insultos con excepción para `#general`, alerta anti-raid, bienvenida, verificación por botón, registros, tickets privados, FAQ, niveles y cupones internos.

## 1. Requisitos
- Node.js 20 o superior.
- Aplicación y bot creados en https://discord.com/developers/applications.
- Activar en **Bot > Privileged Gateway Intents**: **Server Members Intent** y **Message Content Intent**.
- Invitar con los scopes `bot` y `applications.commands`.
- Permisos recomendados: View Channels, Send Messages, Embed Links, Read Message History, Manage Messages, Moderate Members, Kick Members, Ban Members, Manage Channels, Manage Roles. El rol del bot debe estar por encima del rol que asigna como verificado.

## 2. Instalar en Windows
1. Extrae el ZIP.
2. Copia `.env.example` como `.env` y rellena `DISCORD_TOKEN`, `CLIENT_ID` y `GUILD_ID`.
3. Abre PowerShell dentro de la carpeta y ejecuta:
   ```powershell
   npm install
   npm run deploy
   npm start
   ```
4. Mantén el proceso activo en tu PC o hosting.

No compartas el token ni subas `.env` a GitHub. Si el token se filtra, resetea el token en el Developer Portal.

## 3. Configuración inicial
Crea canales de texto `#logs`, `#registros`, `#bienvenida`, `#verificacion` y `#general`, además de roles `Verificado` y `Staff`. Activa Modo desarrollador en Discord > Ajustes > Avanzado para copiar IDs.

Ejecuta `/configurar` con estas opciones y el ID correspondiente:
- `canal-logs`, `canal-registros`, `canal-bienvenida`, `canal-verificacion`, `canal-general`
- `rol-verificado`, `rol-staff`
- `anti-raid` = `on`/`off`
- `automod` = `on`/`off`
- `filtro-insultos` = `on`/`off`
- `umbral-anti-raid` = número 3–30 de entradas en 15 segundos
- `acción-anti-raid` = `alert` o `timeout`

Después ejecuta `/panel-verificacion` y `/panel-tickets`. Comprueba `/estado`.

## 4. Funciones
- Slash commands en español: `/ayuda`, `/configurar`, `/estado`, `/ban`, `/unban`, `/expulsar`, `/timeout`, `/unmute`, `/aviso`, `/advertencias`, `/limpiar`, `/panel-verificacion`, `/panel-tickets`, `/ticket-cerrar`, `/faq`, `/faq-config`, `/nivel`, `/cupongenerar`, `/anuncio`, `/evento`.
- Anti-menciones `@everyone` y `@here`; filtro de insultos fuera del canal general configurado.
- Anti-raid por ráfaga de entradas en 15 segundos. Acción `alert` solo registra; `timeout` intenta aplicar un timeout de 10 minutos a los nuevos miembros cuando se alcanza el umbral. Ajusta el umbral con prudencia y prueba primero en un servidor de test.
- Bienvenidas, salidas, mensajes editados/eliminados, entradas y verificaciones en los canales configurados.
- Identificación del invitador mediante comparación de usos de invitaciones cuando Discord y los permisos lo permiten.
- Tickets privados de compra, venta y soporte; un usuario no puede abrir varios tickets a la vez.
- FAQ inicial sobre PayPal, Paysafecard, criptomonedas, compras, ventas, garantía y soporte. Personaliza con `/faq-config`. Las preguntas relevantes sin respuesta automática pueden derivarse al staff con un límite de una notificación por usuario cada 5 minutos.
- XP persistente, niveles y cupones internos de 1 € y 2 € en hitos de nivel.
- Registros JSON persistentes en `data/store.json`.

## 5. Limitaciones que debes conocer
- Anti-raid: no existe detección perfecta. La acción `timeout` puede afectar a nuevos miembros legítimos si hay muchas entradas; comienza con `alert` y pruébalo antes de activar `timeout`.
- Invitaciones: Discord no permite identificar siempre al invitador (vanity URL, invitaciones privadas, permisos o carreras entre usos); puede salir como no disponible.
- Cupones: se generan y guardan en la base de datos del bot, pero **no descuentan dinero automáticamente**. Para eso hay que integrarlos con el checkout de KENZ SHOP.
- Eventos: `/evento` publica un anuncio, no crea un evento nativo de Discord.
- XP: se concede por mensajes con límite de una recompensa por minuto; no mide actividad de voz.
- El filtro de insultos es una lista básica, no detecta todas las variantes y puede necesitar ajustes.
- La base JSON está pensada para un bot en una sola instancia. No ejecutes dos procesos simultáneos escribiendo el mismo archivo.
- El canal de registros puede contener identificadores de usuarios. Restringe su acceso al personal autorizado e informa a los miembros de qué datos registras.

## 6. Solución de problemas
- Si los comandos no aparecen, vuelve a ejecutar `npm run deploy` y comprueba `CLIENT_ID`, `GUILD_ID`.
- Si el bot no asigna el rol, mueve el rol del bot por encima de `Verificado` y habilita Manage Roles.
- Si no borra mensajes o modera, revisa permisos y jerarquía de roles.
- Si no detecta mensajes, activa Message Content Intent en Developer Portal.
