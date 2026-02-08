# Sesion 6: Instalacion, Configuracion y Seguridad - Guion de Narracion

**Duracion estimada:** ~6 minutos
**Narrador:** Tono amigable, claro, como un tutorial tech en YouTube

---

## [00:00] Apertura

Hola! Bienvenidos a la Sesion 6, la sesion final del curso Learning OpenClaw.

Llegamos al momento de la verdad. Durante cinco sesiones aprendimos la teoria: que es OpenClaw, como funciona la arquitectura, los canales, los agentes, la memoria, las herramientas, los skills, el sandbox. Ahora vamos a aterrizar todo eso en la practica.

En esta sesion vamos a cubrir como instalar OpenClaw, como configurarlo, y como asegurarlo para uso en produccion. Al terminar, vas a tener todo lo que necesitas para desplegar tu propio asistente de IA personal.

Vamos con todo!

---

## [00:35] Metodos de instalacion

Hay tres formas de instalar OpenClaw:

**La primera y recomendada: npm.** Si ya tienes Node.js instalado, es tan simple como ejecutar `npm install -g openclaw`. Esto te da el comando global `openclaw` listo para usar. Es la forma mas rapida de empezar.

> **Nota de pronunciacion:** "npm" se pronuncia deletreado: "en pe eme". Es el gestor de paquetes de Node.js.

**La segunda: desde el codigo fuente.** Clonas el repositorio de GitHub con git clone, entras al directorio, ejecutas npm install y npm run build. Esta opcion es para quienes quieren contribuir al proyecto o hacer builds personalizados.

**Y la tercera: Docker.** Ejecutas `docker pull openclaw/openclaw` y tienes un despliegue aislado y reproducible. Ideal para produccion y para quienes prefieren contenedores.

Las tres opciones te llevan al mismo resultado. Elige la que mejor se adapte a tu flujo de trabajo.

---

## [01:20] Requisitos previos

Antes de instalar, asegurate de tener lo siguiente:

**Node.js version 20 o superior.** Puedes verificarlo con `node --version`. Si necesitas instalarlo, ve a nodejs.org.

**npm version 9 o superior.** Viene incluido con Node.js. Verifica con `npm --version`.

Y **al menos una clave de API de un proveedor de IA.** Puede ser de OpenAI -- esas empiezan con "sk-" --, de Anthropic -- que empiezan con "sk-ant-" --, de Google Gemini, o si quieres correr todo localmente sin ninguna clave, puedes usar Ollama, que ejecuta modelos directamente en tu maquina.

Un tip rapido para verificar: corre `node --version` y `npm --version` en tu terminal. Si ambos estan en las versiones correctas, estas listo.

---

## [02:00] Recorrido del asistente de configuracion

Cuando ejecutas `openclaw` por primera vez, se lanza automaticamente un asistente de configuracion -- el "onboarding wizard".

> **Nota de pronunciacion:** "Onboarding wizard" se pronuncia "onbording uisard" y significa asistente de incorporacion.

El proceso es muy sencillo, tiene cuatro pasos:

**Paso 1:** Elige tu proveedor de IA -- OpenAI, Anthropic, Ollama, u otro.

**Paso 2:** Ingresa tu clave de API.

**Paso 3:** Elige un canal para empezar -- Web es el valor por defecto y el mas facil porque no requiere configuracion externa.

**Paso 4:** Listo! El Gateway se inicia en el puerto 18789 y el chat web esta disponible en localhost puerto 3000.

En cuestion de minutos tienes un asistente de IA funcionando. El asistente genera un archivo openclaw.json5 que puedes editar despues para ajustar la configuracion a tu gusto.

---

## [02:50] Archivo de configuracion en detalle

El archivo openclaw.json5 es el corazon de tu configuracion. Tiene tres secciones principales:

**Gateway** -- donde defines el puerto y el host. Por defecto, el puerto es 18789.

**Agents** -- donde configuras tus agentes de IA. Como minimo necesitas un agente default con un modelo y un proveedor.

**Channels** -- donde defines los canales de mensajeria. El mas simple es el canal web con solo un tipo y un puerto.

Esa es la configuracion minima para empezar. Desde ahi puedes ir agregando complejidad segun la necesites.

---

## [03:20] Caracteristicas de configuracion

El sistema de configuracion tiene cuatro caracteristicas que lo hacen especialmente potente:

**JSON5** -- permite comentarios, comas al final, y claves sin comillas. Si alguna vez sufriste con JSON puro, esto te va a encantar. Poder dejar comentarios explicando que hace cada seccion es invaluable.

**Recarga en caliente** -- o "hot reload". Editas el archivo, guardas, y los cambios se aplican inmediatamente. Sin reiniciar el Gateway. En produccion, esto significa cero tiempo de inactividad para cambios de configuracion.

> **Nota de pronunciacion:** "Hot reload" se pronuncia "jot riloud" y significa recarga en caliente.

**$include** -- la directiva de inclusion. Te permite dividir una configuracion grande en multiples archivos. Por ejemplo, puedes tener un archivo separado para agentes, otro para canales, otro para herramientas. Mucho mas organizado.

Y **sustitucion de variables de entorno** -- referencia variables de entorno directamente en la configuracion usando el prefijo de signo de dolar. Por ejemplo, `$OPENAI_API_KEY`. Nunca, nunca, nunca escribas claves de API directamente en el archivo de configuracion.

---

## [04:05] Panorama de seguridad

Hablemos de seguridad. OpenClaw adopta un enfoque de **defensa en profundidad** -- multiples capas independientes que trabajan juntas para proteger tu sistema.

> **Nota de pronunciacion:** "Defense in depth" se pronuncia "difens in depz" y significa defensa en profundidad.

Las cuatro capas son:

**Politicas de MD** -- controlan quien puede hablar con tu agente. Es la primera barrera.

**Sandbox** -- aisla la ejecucion de herramientas. Impide que codigo malicioso acceda a tu sistema.

**Conciencia de inyeccion de prompt** -- protecciones contra ataques de manipulacion.

**Control de acceso** -- permisos granulares sobre que herramientas puede usar cada agente.

Estas capas funcionan de forma independiente. Incluso si una es comprometida, las otras siguen protegiendo tu sistema.

---

## [04:45] Repaso de politicas de MD

Repasemos rapidamente las politicas de mensajes directos, que ya vimos en la Sesion 3:

**Pairing** -- los usuarios nuevos deben enviar un codigo de verificacion. Recomendado para uso personal.

**Allowlist** -- solo usuarios pre-aprobados pueden chatear. Ideal para equipos.

**Open** -- cualquiera puede chatear. Solo para bots publicos o demos.

**Disabled** -- sin mensajes directos. Solo funciona en grupos.

Mi recomendacion para empezar: usa pairing. Es seguro y la experiencia de usuario es simple -- el usuario ingresa un codigo una vez y listo.

---

## [05:05] Conciencia de inyeccion de prompt

La inyeccion de prompt es un riesgo real y serio para cualquier asistente de IA que puede tomar acciones.

> **Nota de pronunciacion:** "Prompt injection" se pronuncia "prompt inyecshon".

Que es? Un atacante envia un mensaje disenado para manipular al agente, hacer que ignore sus instrucciones y ejecute acciones no deseadas.

OpenClaw incluye protecciones integradas: aislamiento del system prompt, validacion de llamadas a herramientas, filtrado de salida, y aislamiento del sandbox para herramientas de ejecucion.

Pero ninguna proteccion es perfecta. Por eso las mejores practicas son: restringir los permisos de herramientas al minimo necesario, habilitar el sandbox en produccion, revisar los logs del agente regularmente, usar politicas de allowlist cuando sea posible, y tener cuidado especial con herramientas que acceden a contenido externo.

---

## [05:45] Herramienta de diagnostico Doctor

OpenClaw incluye una herramienta de diagnostico genial: `openclaw doctor`.

Cuando algo no funciona, es lo primero que debes ejecutar. Verifica tu archivo de configuracion -- sintaxis valida, estructura correcta. Comprueba los proveedores de IA -- claves configuradas, APIs accesibles. Prueba los canales -- tokens validos, servicios alcanzables. Y valida las herramientas -- que Chromium este disponible para el browser, que el sandbox este habilitado.

Te da un resumen claro: cuantas verificaciones pasaron, cuantas advertencias hay, y cuantas fallaron. Es tu mejor amigo cuando necesitas diagnosticar problemas.

---

## [06:05] Referencia de comandos CLI

Repaso rapido de los comandos esenciales:

`openclaw` -- sin argumentos, inicia el sistema. En la primera ejecucion lanza el asistente de configuracion.

`openclaw doctor` -- ejecuta diagnosticos de configuracion, conectividad y salud.

`openclaw config` -- muestra la configuracion actual fusionada, util cuando usas $include.

`openclaw --version` -- muestra la version instalada.

Y `openclaw --help` -- para ver todos los comandos y opciones disponibles.

---

## [06:25] Consejos de despliegue

Para llevar OpenClaw a produccion, cuatro recomendaciones:

**Usa un gestor de procesos** como pm2 o systemd. Esto asegura que OpenClaw se mantenga corriendo y se reinicie automaticamente si hay un fallo.

> **Nota de pronunciacion:** "pm2" se pronuncia "pi em dos".

**Pon un proxy inverso** como nginx o Caddy al frente. Esto te da terminacion SSL, balanceo de carga, y servicio de archivos estaticos.

**Siempre usa HTTPS.** Nunca expongas el Gateway sobre HTTP plano. Usa Let's Encrypt para certificados gratuitos.

Y **haz respaldos regulares** de tu directorio workspace. Ahi viven las conversaciones, los skills personalizados, y el estado de los agentes. Un simple `tar -czf backup.tar.gz workspace/` es suficiente.

---

## [06:55] Curso completado! / Resumen final

Y con eso... hemos completado las seis sesiones del curso Learning OpenClaw!

Hagamos un repaso rapido de todo lo que cubrimos:

**Sesion 1** -- que es OpenClaw, su propuesta de valor y el panorama general.

**Sesion 2** -- la arquitectura interna, el Gateway como cerebro del sistema, y la comunicacion via WebSocket.

**Sesion 3** -- los canales, como conectar mas de trece plataformas, politicas de MD, y enrutamiento multi-agente.

**Sesion 4** -- los agentes como personalidades de IA aisladas, el sistema de memoria con notas diarias y MEMORY.md, y la gestion de sesiones.

**Sesion 5** -- las herramientas que dan capacidades reales a los agentes, los skills como paquetes reutilizables, y el sandbox para seguridad.

**Sesion 6** -- instalacion, configuracion con JSON5, seguridad en profundidad, y despliegue en produccion.

Ahora tienes el mapa completo de OpenClaw. El siguiente paso es ponerte manos a la obra: instala OpenClaw, construye tu propio agente, crea skills personalizados, y unete a la comunidad.

---

## [07:30] Cierre / Recursos

Para continuar tu viaje, aqui tienes los recursos esenciales:

La guia de inicio rapido en **docs.openclaw.ai/getting-started** -- instrucciones paso a paso para la configuracion completa.

La documentacion de seguridad en **docs.openclaw.ai/security** -- todo lo que necesitas saber para asegurar tu despliegue.

La documentacion de Docker en **docs.openclaw.ai/docker** -- para despliegues con contenedores, incluyendo ejemplos de Docker Compose.

Y la comunidad en **docs.openclaw.ai/help** -- para obtener ayuda, reportar errores y participar en discusiones.

Muchas gracias por acompanarme durante estas seis sesiones. Espero que el curso te haya dado una comprension solida y practica de como funciona OpenClaw.

Ahora ve y construye algo increible!

Hasta la proxima!

---

*Fin del guion - Sesion 6*
*Fin del curso Learning OpenClaw*
