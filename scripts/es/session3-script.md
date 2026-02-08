# Sesion 3: Canales y Multi-Plataforma - Guion de Narracion

**Duracion estimada:** ~5 minutos
**Narrador:** Tono amigable, claro, como un tutorial tech en YouTube

---

## [00:00] Apertura

Hola! Bienvenidos a la Sesion 3 del curso Learning OpenClaw.

Ya conocemos la arquitectura general y sabemos que el Gateway es el cerebro del sistema. Ahora vamos a hablar de algo que en lo personal me parece una de las caracteristicas mas atractivas de OpenClaw: los canales.

Imaginate esto: tienes un solo asistente de IA, y puedes hablarle por WhatsApp mientras estas en el telefono, por Telegram cuando estas en la computadora, por Discord cuando estas en tu comunidad, y por una interfaz web cuando estas en el trabajo. Todo con el mismo contexto, la misma memoria, el mismo asistente.

Eso es lo que hacen los canales. Vamos a ver como funcionan.

---

## [00:40] Que son los canales?

Un canal es basicamente un conector a una plataforma de mensajeria. Su trabajo es traducir los mensajes especificos de cada plataforma al formato interno de OpenClaw, y viceversa.

> **Nota de pronunciacion:** "Channel" se pronuncia "chanel" en ingles y significa canal.

Cada canal se encarga de cuatro cosas principales:

Primero, maneja la conexion con la plataforma -- ya sea WhatsApp, Telegram, o la que sea.

Segundo, normaliza los mensajes en un formato unificado. Esto es clave porque el resto del sistema no necesita saber si el mensaje vino de WhatsApp o de Discord -- lo ve todo igual.

Tercero, gestiona la autenticacion con las APIs externas -- tokens de bot, claves de API, etcetera.

Y cuarto, maneja las caracteristicas especificas de cada plataforma, como reacciones, hilos, botones, stickers... lo que sea que la plataforma soporte.

---

## [01:25] Lista de 13+ plataformas soportadas

Y hablando de plataformas, la lista es impresionante. OpenClaw soporta mas de trece plataformas de forma nativa:

WhatsApp usando la libreria Baileys. Telegram con Grammy. Discord con discord.js. Slack con Bolt. Un canal Web integrado. Matrix con matrix-js-sdk. Signal -- aunque todavia en beta -- con signal-cli. IRC con irc-framework. XMPP con stanza.io. Nostr -- tambien en beta -- con nostr-tools. SMS a traves de Twilio. Line con el SDK oficial. Y un canal de webhook personalizado que te permite integrarte con basicamente cualquier cosa que pueda enviar solicitudes HTTP.

Cada plataforma usa una libreria bien mantenida, asi que la integracion es solida y confiable.

---

## [02:15] Arquitectura de canales

Visualmente, la arquitectura de canales es bastante clara.

Los dispositivos del usuario -- telefono con WhatsApp, telefono con Telegram, computadora con Discord, navegador con la interfaz web -- cada uno se conecta a su canal correspondiente dentro de un nodo. Y todos esos canales alimentan un unico enrutador dentro del Gateway.

El enrutador es el que decide que agente debe manejar cada mensaje.

Un detalle importante: cada canal se ejecuta de forma independiente. Si el canal de WhatsApp tiene un problema, Telegram y Discord siguen funcionando sin ninguna interrupcion. Esto es crucial para la confiabilidad del sistema.

---

## [02:50] Ejemplo de configuracion

Configurar un canal es sorprendentemente sencillo. En tu archivo de configuracion JSON5, defines un bloque de canales con un nombre unico para cada uno.

Por ejemplo, puedes tener "mi-telegram" de tipo "telegram" con su token de bot referenciado como variable de entorno -- nunca escribas tokens directamente en el archivo, siempre usa variables de entorno con el signo de dolar. Luego "mi-discord" de tipo "discord" con su propio token. Y una "interfaz-web" de tipo "web" en el puerto 3100.

Cada canal tambien tiene una configuracion llamada dmPolicy -- que se pronuncia "di em polici" -- y que controla quien puede enviar mensajes directos al bot. Eso es exactamente lo que vamos a ver ahora.

> **Nota de pronunciacion:** "dmPolicy" significa "politica de mensajes directos". DM viene de "Direct Message".

---

## [03:30] Politicas de MD

Las politicas de mensajes directos -- o politicas de MD -- son una caracteristica clave de seguridad. Hay cuatro modos:

**Pairing** -- el modo por defecto recomendado. El usuario debe ingresar un codigo de verificacion unico para demostrar que tiene acceso al panel de OpenClaw. Es ideal para uso personal o familiar.

> **Nota de pronunciacion:** "Pairing" se pronuncia "pering" y significa emparejamiento.

**Allowlist** -- solo los IDs de usuario pre-aprobados pueden enviar mensajes. Util para equipos y organizaciones donde conoces el ID de cada persona en la plataforma.

> **Nota de pronunciacion:** "Allowlist" se pronuncia "alau list" y significa lista de permitidos.

**Open** -- cualquiera puede enviar mensajes al bot. Solo deberias usar esto para bots publicos o demos.

**Disabled** -- no se permiten mensajes directos en absoluto. El bot solo funciona en grupos.

Mi recomendacion: empieza con pairing. Ofrece buena seguridad con una experiencia de usuario simple y amigable.

---

## [04:10] Enrutamiento multi-agente

Ahora, cuando llega un mensaje, como decide OpenClaw que agente debe manejarlo?

Usa una cadena de prioridad con tres niveles:

**Prioridad uno: coincidencia exacta.** Si hay un mapeo directo para ese canal y usuario o grupo especifico, ese agente lo maneja.

**Prioridad dos: canal por defecto.** Si no hay coincidencia exacta, se usa el agente por defecto configurado para ese canal.

**Prioridad tres: global por defecto.** Si nada coincide, el agente de respaldo a nivel del sistema se encarga.

Esto te permite tener diferentes agentes para diferentes contextos sin configuracion compleja. Por ejemplo, tu canal de Discord podria usar un agente tecnico, mientras que tu canal de WhatsApp usa un asistente mas casual.

---

## [04:45] Soporte de chat grupal y medios

Hablemos brevemente de dos temas mas: chat grupal y soporte de medios.

Para grupos, el bot se une como miembro y puede configurarse para responder de tres maneras: solo cuando alguien lo menciona con arroba, cuando se usan palabras clave de activacion como "ayuda" o "pregunta", o siempre -- respondiendo a cada mensaje.

En cuanto a medios, no todas las plataformas soportan lo mismo. WhatsApp, Telegram, Discord y Slack soportan practicamente todo: imagenes, audio, archivos, botones y reacciones. IRC, por ejemplo, es solo texto. Pero OpenClaw maneja esto de forma elegante: si un agente intenta enviar un boton a IRC, automaticamente recurre a un menu basado en texto. Tu codigo de agente no necesita preocuparse por las diferencias entre plataformas.

---

## [05:10] Canal web

Un ultimo canal que merece mencion especial es el canal web. Es frecuentemente el primero que los usuarios configuran porque no requiere ninguna configuracion externa. No necesitas crear un bot de Telegram, ni registrarte en WhatsApp Business, ni nada de eso.

Solo defines el tipo como "web", eliges un puerto, y listo. Abres tu navegador y ya tienes un chat funcionando. Ademas, la interfaz es responsiva, personalizable, soporta markdown, subida de archivos, y se puede embeber en otras aplicaciones web mediante iframe.

---

## [05:35] Conclusiones clave

Tres cosas para recordar:

**Uno:** los canales abstraen toda la complejidad de las plataformas. Escribe la logica de tu agente una vez y despliegala a mas de trece plataformas sin cambios.

**Dos:** las politicas de MD controlan quien puede hablar con tu asistente. Es tu primera linea de defensa.

**Tres:** el enrutamiento te permite dirigir mensajes al agente correcto automaticamente usando una cadena de prioridad simple.

---

## [05:55] Cierre / Recursos

Y eso es todo para la Sesion 3!

Si quieres profundizar, la documentacion de canales y la referencia de politicas de MD estan disponibles en la documentacion oficial de OpenClaw.

En la proxima sesion -- la Sesion 4 -- vamos a explorar los agentes en profundidad. Vamos a ver como crear personalidades de IA con memoria persistente, como funciona el sistema de memoria, y como se gestionan las sesiones.

Es donde las cosas se ponen realmente interesantes. Nos vemos ahi!

---

*Fin del guion - Sesion 3*
