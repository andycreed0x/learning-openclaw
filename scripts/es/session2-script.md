# Sesion 2: Arquitectura y Gateway - Guion de Narracion

**Duracion estimada:** ~5 minutos
**Narrador:** Tono amigable, claro, como un tutorial tech en YouTube

---

## [00:00] Apertura

Hola de nuevo! Bienvenidos a la Sesion 2 del curso Learning OpenClaw.

En la sesion anterior vimos el panorama general: que es OpenClaw, que problema resuelve, y cual es su propuesta de valor. Hoy vamos a abrir el capo y meternos en la arquitectura interna. Vamos a entender como estan organizados los componentes y, sobre todo, vamos a profundizar en el Gateway, que es el cerebro de todo el sistema.

Si alguna vez te has preguntado "pero como funciona esto por dentro?", esta sesion es para ti.

---

## [00:30] Los 6 componentes centrales

OpenClaw se compone de seis piezas fundamentales. Vamos a repasarlas rapidamente:

El **Gateway** es el orquestador central. Todo el trafico pasa por el. Piensa en el como el cerebro del sistema.

Los **Canales** son los conectores a plataformas de mensajeria: WhatsApp, Telegram, Discord, Slack, y muchos mas.

Los **Agentes** son las personalidades de IA. Cada agente tiene su propio modelo, sus instrucciones, y sus herramientas.

Los **Nodos** -- en ingles "nodes" -- son las instancias que ejecutan los canales y se conectan al Gateway.

> **Nota de pronunciacion:** "Nodes" se pronuncia "nods" y significa nodos.

Las **Sesiones** mantienen el contexto y la memoria de cada conversacion entre un usuario y un agente.

Y las **Herramientas** son las capacidades externas que los agentes pueden usar: un navegador web, ejecucion de codigo, tareas programadas, y mas.

Estas seis piezas trabajan juntas para crear una plataforma completa de asistente de IA.

---

## [01:20] Recorrido del diagrama de arquitectura completo

Ahora veamos como se conectan estas piezas en el diagrama completo.

En la parte superior estan los canales: WhatsApp, Telegram, Discord, Web, Slack. Todos se conectan al servidor WebSocket del Gateway en el puerto 18789. Dentro del Gateway hay tres componentes clave: el servidor WebSocket que recibe las conexiones, el enrutador que decide que agente maneja cada mensaje, y el gestor de sesiones que mantiene el contexto.

Debajo del Gateway estan los agentes -- por ejemplo, Agente 1 y Agente 2 -- cada uno con su propia configuracion y modelo de IA. Y en la parte inferior, las herramientas: navegador, ejecucion de codigo, tareas programadas con cron, entre otras.

Todo se comunica a traves del Gateway. Es el punto central por donde fluye absolutamente todo.

---

## [02:10] Gateway en detalle

Hablemos del Gateway mas a fondo, porque es realmente el corazon del sistema.

Lo primero: funciona como un plano de control WebSocket. Escucha en el puerto 18789 por defecto, y todas las comunicaciones internas pasan por ahi.

> **Nota de pronunciacion:** "WebSocket" se pronuncia "web soket".

Una de sus caracteristicas mas potentes es la **recarga en caliente** -- o "hot reload" en ingles. Esto significa que puedes modificar el archivo de configuracion y los cambios se aplican inmediatamente, sin necesidad de reiniciar el servicio. En produccion, esto es invaluable.

El Gateway tambien actua como orquestador: gestiona el enrutamiento de mensajes, las sesiones activas, y las conexiones de todos los nodos.

Y mantiene un estado centralizado: sabe exactamente que nodos estan conectados, que canales estan activos, y que agentes estan disponibles.

---

## [03:00] Flujo de solicitud paso a paso

Vamos a caminar por el flujo completo de una solicitud, paso a paso.

Paso uno: un usuario envia un mensaje -- digamos "Hola, que tiempo hace hoy?" -- a traves de WhatsApp.

Paso dos: el canal de WhatsApp recibe el mensaje y lo transmite al Gateway via WebSocket.

Paso tres: el Gateway recibe el mensaje, identifica la sesion del usuario, y lo enruta al agente correspondiente usando la cadena de prioridad que veremos en la Sesion 3.

Paso cuatro: el agente recibe el mensaje, lo procesa usando su modelo de IA, y si necesita informacion adicional, llama a una herramienta -- en este caso podria usar el navegador para buscar el clima.

Paso cinco: la respuesta fluye de regreso por el mismo camino -- del agente al Gateway, del Gateway al canal, y del canal al usuario.

Todo el viaje de ida y vuelta sucede en cuestion de segundos.

---

## [03:45] Stack tecnologico

Hablemos brevemente del stack tecnologico, para que tengas el mapa completo.

El **runtime** es Node.js con TypeScript, que proporciona rendimiento y seguridad de tipos.

> **Nota de pronunciacion:** "Runtime" se pronuncia "rantaim" y se refiere al entorno de ejecucion.

El **protocolo** de comunicacion es WebSocket -- bidireccional y en tiempo real.

La **configuracion** usa formato JSON5 -- que es como JSON pero con comentarios y comas al final permitidas. Mucho mas amigable para humanos.

Para **IA**, es multi-proveedor: OpenAI, Anthropic, Gemini, Ollama, y mas.

La **base de datos** es SQLite -- simple, sin necesidad de un servidor de base de datos separado.

Y el **sandbox** usa isolated-vm para ejecucion segura de codigo aislado.

> **Nota de pronunciacion:** "Sandbox" se pronuncia "sandboxs" y significa caja de arena -- un entorno aislado y seguro.

---

## [04:15] Configuracion con JSON5

La configuracion de OpenClaw vive en un archivo llamado openclaw.json -- o openclaw.json5 -- usando el formato JSON5.

Que tiene de especial JSON5? Permite comentarios -- eso solo ya es un cambio de vida si has trabajado con JSON puro. Permite comas al final. Y permite claves sin comillas.

En el archivo defines la configuracion del Gateway, los agentes disponibles, y puedes usar la directiva **$include** -- con signo de dolar -- para dividir la configuracion en multiples archivos. Esto es genial cuando la configuracion crece.

Y recuerda: al guardar cambios, se recargan automaticamente sin reiniciar. Hot reload.

---

## [04:45] Arquitectura multi-nodo y nodos explicados

Para cerrar los conceptos de arquitectura, hablemos de nodos.

Un **nodo** es simplemente una instancia de OpenClaw que ejecuta canales. Se conecta al Gateway via WebSocket y puede correr en cualquier maquina.

En el caso mas simple, tienes un solo nodo en la misma maquina que el Gateway. Todo junto, facil de manejar.

Pero OpenClaw tambien soporta una arquitectura multi-nodo. Puedes tener el Gateway en la nube, un nodo en tu casa corriendo WhatsApp y Telegram, otro nodo en una VPS corriendo Discord y Slack, y otro nodo mas en algun otro servidor corriendo la interfaz web y Matrix.

Todos comparten los mismos agentes y sesiones a traves del Gateway. Esto te da flexibilidad para distribuir la carga, tener redundancia, o simplemente organizar tus canales como prefieras.

---

## [05:15] Conclusiones clave

Tres puntos clave para recordar de esta sesion:

**Primero**, el Gateway es el cerebro. Todo el trafico pasa por el. Es el orquestador central que enruta mensajes, gestiona sesiones y coordina agentes.

**Segundo**, todo se conecta via WebSocket. Un protocolo unico y bidireccional para toda la comunicacion interna. Simple, eficiente y en tiempo real.

**Tercero**, JSON5 con recarga en caliente. La configuracion es legible, modular con $include, y se aplica sin reiniciar el sistema. Esto hace que ajustar el sistema sea rapido y sin interrupciones.

---

## [05:40] Cierre / Recursos

Y eso es todo para la Sesion 2!

Si quieres profundizar mas, te recomiendo la documentacion de arquitectura en **docs.openclaw.ai/architecture** y la documentacion del Gateway en **docs.openclaw.ai/gateway**. La referencia de configuracion esta en **docs.openclaw.ai/configuration**.

En la proxima sesion -- la Sesion 3 -- vamos a explorar los canales y como conectar OpenClaw a multiples plataformas de mensajeria. Es donde las cosas se ponen realmente interesantes para el uso del dia a dia.

Nos vemos ahi!

---

*Fin del guion - Sesion 2*
