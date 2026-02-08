# Sesion 4: Agentes, Memoria y Sesiones - Guion de Narracion

**Duracion estimada:** ~5 minutos
**Narrador:** Tono amigable, claro, como un tutorial tech en YouTube

---

## [00:00] Apertura

Hola! Bienvenidos a la Sesion 4 del curso Learning OpenClaw.

Esta sesion es, para mi, donde OpenClaw se vuelve verdaderamente poderoso. Vamos a hablar de agentes -- que son las personalidades de IA que tu creas --, del sistema de memoria que les permite recordar cosas entre conversaciones, y de las sesiones que gestionan el ciclo de vida de cada interaccion.

Si alguna vez quisiste un asistente de IA que realmente te conozca, que recuerde tus preferencias y que sepa en que estas trabajando sin tener que repetirle todo cada vez... esto es exactamente de lo que se trata.

Vamos!

---

## [00:35] Que es un agente?

Un agente en OpenClaw es una personalidad de IA con su propio espacio de trabajo, herramientas y memoria. Piensa en cada agente como un empleado independiente con su propio escritorio, sus archivos y su area de experiencia.

Cada agente tiene su propio **system prompt** -- es decir, las instrucciones del sistema que le dicen como comportarse. Mantiene **memoria separada** por usuario. Puede acceder a diferentes **herramientas y skills**. Y se ejecuta en un **entorno aislado**.

> **Nota de pronunciacion:** "System prompt" se pronuncia "sistem prompt" y se refiere a las instrucciones base que recibe el modelo de IA.

Un concepto fundamental: los agentes **no comparten memoria ni contexto**. Lo que un agente sabe, el otro no lo sabe. Esto es intencional. Mantiene las responsabilidades separadas y previene fugas de datos entre contextos diferentes.

---

## [01:20] Estructura del espacio de trabajo

Toda la configuracion de un agente vive en archivos dentro de un directorio. No hay bases de datos ni paneles de administracion para esto -- son simplemente archivos markdown en una carpeta.

> **Nota de pronunciacion:** "Workspace" se pronuncia "work speis" y significa espacio de trabajo.

La estructura es asi: tienes un directorio workspace, dentro un directorio agents, y dentro de ese un directorio para cada agente. Por ejemplo, "my-agent". Dentro de ese directorio encuentras varios archivos markdown -- y en un momento vamos a ver cada uno.

Tambien tienes un directorio de skills para habilidades personalizadas, y un directorio de sandbox para la ejecucion aislada.

Lo bonito de que todo sea archivos es que puedes usar control de versiones con git, hacer code review de los cambios en las personalidades, y colaborar con tu equipo de forma natural.

---

## [02:05] Archivos de personalidad explicados

Veamos los archivos clave que definen la personalidad de un agente:

**AGENTS.md** -- este es el principal. Aqui defines el system prompt, las reglas de comportamiento, y el estilo de respuesta. Es la instruccion central que le dice al modelo de IA quien es y como debe actuar.

**SOUL.md** -- este le da sabor a la personalidad. Define rasgos como: es formal o casual? Es serio o tiene humor? Prefiere respuestas cortas o detalladas? Es lo que hace que cada agente se sienta unico y consistente.

> **Nota de pronunciacion:** "Soul" se pronuncia "soul" y significa alma.

**TOOLS.md** -- controla que herramientas puede usar el agente y como. Define las capacidades y los permisos.

**USER.md** -- este se genera automaticamente para cada usuario. Almacena preferencias e informacion sobre interacciones pasadas.

Y **MEMORY.md** -- la memoria de largo plazo que persiste a traves de todas las sesiones. De este vamos a hablar mucho mas en un momento.

---

## [02:55] Configuracion multi-agente

Un caso de uso tipico es tener multiples agentes, cada uno especializado en algo diferente.

Por ejemplo, puedes tener un agente **Asistente** de proposito general para preguntas del dia a dia. Un agente **Desarrollador** con herramientas de codigo y DevOps habilitadas. Y un agente **Analista** con acceso a datos y capacidad de generar reportes.

Cada uno vive en su propio directorio de workspace. Puedes enrutar mensajes a cada agente segun el canal o las reglas de enrutamiento que vimos en la sesion anterior.

Y aqui viene algo genial: puedes asignar diferentes modelos de IA a cada agente. Quizas un modelo economico para el asistente general, y uno mas capaz -- y mas caro -- para el agente de desarrollo donde la calidad del codigo importa mas. Esto te permite optimizar el costo sin sacrificar la experiencia.

---

## [03:40] Sistema de memoria

Ahora hablemos de lo que realmente hace que los agentes de OpenClaw se sientan especiales: el sistema de memoria.

Hay dos tipos de memoria que trabajan juntos:

Las **notas diarias** son la memoria de corto plazo. Se generan automaticamente al final de cada conversacion. Capturan los hechos clave, las decisiones tomadas, y la informacion importante. Estan organizadas por fecha y sirven como insumo para la memoria de largo plazo.

> **Nota de pronunciacion:** "Daily notes" se pronuncia "deili nouts" y significa notas diarias.

**MEMORY.md** es la memoria de largo plazo. Es persistente a traves de todas las sesiones. Se compacta a partir de las notas diarias. Contiene preferencias del usuario, hechos importantes, y todo lo que el agente necesita recordar. Y lo mas importante: se carga al inicio de cada nueva sesion.

---

## [04:15] Flujo de memoria

El flujo funciona asi:

Paso uno: tienes una conversacion con tu agente.

Paso dos: al terminar, el sistema extrae automaticamente los puntos clave y los guarda como notas diarias.

Paso tres: periodicamente, un proceso de **compactacion** toma todas las notas diarias acumuladas, extrae los hechos importantes, elimina informacion redundante u obsoleta, y actualiza MEMORY.md con el conocimiento destilado.

> **Nota de pronunciacion:** "Compaction" se pronuncia "compaction" y significa compactacion o condensacion.

Paso cuatro: la proxima vez que inicias una sesion, MEMORY.md se carga como contexto. Tu agente ya sabe que prefieres el modo oscuro, que tu gato se llama Luna, y que estas trabajando en un proyecto de Python -- sin tener que releer todas las conversaciones anteriores.

Es como tener un asistente que genuinamente te conoce con el tiempo.

---

## [04:55] Sesiones explicadas y diagrama de estado

Las sesiones son el contenedor de las conversaciones. Una sesion es un hilo de conversacion entre un usuario y un agente.

El ciclo de vida es asi: se **crea** cuando un usuario envia un mensaje nuevo. Esta **activa** mientras la conversacion sigue. Pasa a **inactiva** cuando no hay actividad por un periodo configurado. Y **expira** cuando se alcanza el timeout de inactividad.

Hay un estado mas que es importante: **compactada**. Cuando el historial de la conversacion se vuelve demasiado largo y excede el limite configurado, los mensajes mas antiguos se resumen y se reemplazan con una version condensada. Esto evita que la conversacion supere el limite de contexto del modelo de IA.

---

## [05:25] Configuracion de sesiones

En la configuracion, puedes ajustar parametros como:

**idleTimeout** -- cuanto tiempo una sesion permanece activa sin mensajes. Por ejemplo, treinta minutos.

**maxHistory** -- el numero maximo de mensajes antes de que se active la compactacion. Un buen punto de partida es 50.

**compaction** -- la estrategia de compactacion. La mas comun es "summarize", que usa el modelo de IA para crear un resumen de los mensajes antiguos mientras conserva los ultimos N mensajes tal cual.

Y **dailyNotes** -- para controlar si el sistema extrae hechos automaticamente despues de cada sesion.

Un consejo: un maxHistory mas bajo ahorra tokens pero puede perder contexto. Empieza con 50 y ajusta segun tu uso.

---

## [05:55] Conclusiones clave

Tres puntos para llevar:

**Uno:** los agentes son personalidades de IA aisladas. Cada uno tiene su propio espacio de trabajo, herramientas y memoria. No comparten contexto, y eso es una caracteristica, no una limitacion.

**Dos:** la memoria persiste entre sesiones. Las notas diarias capturan el contexto de corto plazo, MEMORY.md proporciona continuidad de largo plazo. Tu agente genuinamente aprende sobre ti con el tiempo.

**Tres:** las sesiones tienen gestion automatica de su ciclo de vida. Compactacion automatica, timeouts de inactividad, y limpieza -- todo funciona sin que tengas que intervenir.

---

## [06:15] Cierre / Recursos

Excelente! Eso fue la Sesion 4.

Para profundizar, revisa la documentacion de agentes, la guia de configuracion multi-agente, y la documentacion del sistema de memoria en la pagina oficial de OpenClaw.

En la proxima sesion -- la Sesion 5 -- vamos a explorar las herramientas, los skills y el sandbox. Ahi es donde tus agentes dejan de ser solo conversacionales y se convierten en asistentes que pueden tomar accion en el mundo real: navegar la web, ejecutar codigo, programar tareas...

Va a estar genial. Nos vemos!

---

*Fin del guion - Sesion 4*
