# Sesion 1: Que es OpenClaw? - Guion de Narracion

**Duracion estimada:** ~4 minutos
**Narrador:** Tono amigable, claro, como un tutorial tech en YouTube

---

## [00:00] Apertura / Bienvenida

Hola, que tal! Bienvenidos al curso Learning OpenClaw. Mi nombre es [NOMBRE DEL NARRADOR] y voy a ser tu guia durante estas seis sesiones donde vamos a explorar a fondo como funciona OpenClaw.

Si eres programador, administrador de sistemas, o simplemente alguien con curiosidad tecnica que quiere entender como armar su propio asistente de inteligencia artificial, estas en el lugar correcto.

En esta primera sesion vamos a responder la pregunta fundamental: que es OpenClaw y por que deberia importarte?

Vamos a ello.

---

## [00:30] El problema con los asistentes de IA actuales

Antes de hablar de la solucion, hablemos del problema.

Hoy en dia, si quieres usar un asistente de IA, tienes que elegir un proveedor: ChatGPT, Claude, Gemini... y quedarte encerrado ahi. Tus conversaciones, tu contexto, tus datos, todo vive en sus servidores. No tienes control.

Ademas, no puedes cambiar de modelo facilmente. Si empezaste con GPT-4 pero quieres probar Claude, tienes que irte a otra plataforma completamente diferente. Pierdes todo el contexto.

Y lo peor: no hay forma de acceder a tu asistente desde multiples plataformas. No puedes hablarle por WhatsApp, luego por Telegram, y que sea la misma conversacion. Cada app es un silo separado.

Eso es lo que OpenClaw viene a resolver.

---

## [01:15] Presentando OpenClaw como la solucion

OpenClaw -- que se pronuncia "open clo" -- es una plataforma de asistente de IA personal que tu mismo alojas en tu propio servidor.

> **Nota de pronunciacion:** "OpenClaw" se pronuncia "open clo", como "open" en ingles y "clo" como la palabra inglesa "claw" que significa garra.

Tiene cuatro pilares fundamentales:

Primero, es **local-first** -- se ejecuta en tu hardware y tus datos son tuyos, punto.

Segundo, es **multi-canal** -- puedes conectarlo a WhatsApp, Telegram, Discord, Slack, una interfaz web, y muchos mas, todos al mismo tiempo. Un solo asistente, todas las plataformas.

Tercero, es **agnostico de modelo** -- funciona con OpenAI, Anthropic, Google Gemini, Ollama para modelos locales, o cualquier API compatible con OpenAI.

Y cuarto, es **codigo abierto** bajo licencia AGPLv3. Puedes inspeccionar el codigo, modificarlo y contribuir.

---

## [02:15] Vista general de la arquitectura

En su forma mas simple, OpenClaw tiene cuatro capas que vamos a ver mucho durante el curso.

Los **usuarios** envian mensajes a traves de **canales** -- que son los conectores a plataformas como WhatsApp o Telegram. Esos mensajes llegan al **Gateway** -- que se pronuncia "gueituei" -- que es el orquestador central. El Gateway los enruta hacia **agentes de IA**, y esos agentes pueden llamar a **herramientas** para navegar la web, ejecutar codigo, programar tareas, y mucho mas.

> **Nota de pronunciacion:** "Gateway" se pronuncia "gueituei" y significa puerta de enlace.

Es una arquitectura limpia y modular que iremos desarmando pieza por pieza en las proximas sesiones.

---

## [02:50] Comparativa de diferenciadores clave

Ahora, para que quede realmente claro por que OpenClaw es diferente, hagamos una comparacion rapida.

Auto-alojado: OpenClaw si, los demas no. Multi-canal: OpenClaw si, los demas no. Eleccion de modelo: OpenClaw te deja usar cualquiera, mientras que ChatGPT solo usa GPT, Claude solo Claude, y Gemini solo Gemini. Codigo abierto: solo OpenClaw. Herramientas extensibles: OpenClaw es completamente extensible, los demas son limitados. Y propiedad de datos: con OpenClaw tus datos son cien por ciento tuyos.

Ningun otro asistente principal ofrece todo esto junto. Esa es la propuesta de valor.

---

## [03:25] Como funciona (flujo simplificado)

Veamos rapidamente como fluye un mensaje por el sistema.

Un usuario envia un mensaje -- digamos por WhatsApp. El canal de WhatsApp recibe ese mensaje y lo reenvia al Gateway via WebSocket -- que se pronuncia "web soket". El Gateway identifica la sesion, determina que agente debe manejar el mensaje, y se lo envia. El agente lo procesa usando su modelo de IA -- por ejemplo GPT-4o -- y si necesita tomar alguna accion, llama a herramientas. La respuesta fluye de regreso por el mismo camino hasta llegar al usuario.

> **Nota de pronunciacion:** "WebSocket" se pronuncia "web soket" y es un protocolo de comunicacion bidireccional en tiempo real.

Todo esto sucede en cuestion de segundos.

---

## [03:55] Modelos de IA soportados

Una de las cosas mas geniales de OpenClaw es la variedad de modelos que soporta.

Puedes usar OpenAI con GPT-4o, GPT-4 o GPT-3.5. Anthropic con Claude Opus, Sonnet o Haiku. Google Gemini Pro y Flash. Ollama para correr modelos locales como Llama, Mistral o Phi completamente offline. OpenRouter que te da acceso a mas de cien modelos con una sola API. Y cualquier servidor compatible con la API de OpenAI, como LM Studio o vLLM.

Incluso puedes usar diferentes modelos para diferentes agentes. Por ejemplo, un modelo economico para tu asistente general y uno mas potente para tu agente de desarrollo.

---

## [04:25] Lo que viene en las proximas sesiones

Este fue solo el panorama general. En las proximas cinco sesiones vamos a profundizar en cada componente.

En la Sesion 2 exploraremos la arquitectura y el Gateway en detalle. La Sesion 3 cubre canales y como lograr la conectividad multi-plataforma. La Sesion 4 se sumerge en agentes, memoria y sesiones. La Sesion 5 trata sobre herramientas, skills y el sandbox. Y la Sesion 6 cierra con instalacion, configuracion y seguridad.

---

## [04:50] Cierre / Recursos

Y con eso cerramos la Sesion 1.

Antes de irte, te dejo tres recursos clave que te recomiendo guardar en marcadores:

La documentacion oficial en **docs.openclaw.ai** -- ahi vas a encontrar la referencia completa.
El repositorio de GitHub en **github.com/openclaw/openclaw** -- dale una estrella si te parece interesante.
Y la pagina de ayuda en **docs.openclaw.ai/help** -- para conectar con la comunidad.

Nos vemos en la Sesion 2, donde vamos a abrir el capo del auto y ver como funciona el Gateway por dentro.

Hasta la proxima!

---

*Fin del guion - Sesion 1*
