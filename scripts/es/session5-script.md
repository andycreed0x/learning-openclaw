# Sesion 5: Herramientas, Skills y Sandbox - Guion de Narracion

**Duracion estimada:** ~5 minutos
**Narrador:** Tono amigable, claro, como un tutorial tech en YouTube

---

## [00:00] Apertura

Hola! Bienvenidos a la Sesion 5 del curso Learning OpenClaw.

Hasta ahora hemos cubierto la arquitectura, los canales, los agentes y la memoria. Todo eso esta genial, pero hay algo que falta: accion. Un agente que solo puede conversar es util, pero un agente que puede navegar la web, ejecutar codigo, programar tareas y automatizar flujos de trabajo... eso es otra cosa completamente diferente.

En esta sesion vamos a hablar de tres conceptos fundamentales: las herramientas, que le dan "manos y ojos" a tus agentes; los skills, que empaquetan funcionalidad reutilizable; y el sandbox, que mantiene todo seguro.

Empecemos.

---

## [00:40] Que son las herramientas?

Las herramientas -- o "tools" en ingles -- son lo que extiende las capacidades de un agente mas alla de la conversacion.

> **Nota de pronunciacion:** "Tools" se pronuncia "tuls" y significa herramientas.

Sin herramientas, un agente solo puede generar texto. Esta limitado a lo que el modelo de IA sabe. Con herramientas, un agente puede navegar la web, leer y escribir archivos, ejecutar codigo, enviar mensajes, programar tareas, y mucho mas.

Piensa en las herramientas como las manos y los ojos de tu agente. Sin ellas solo puede hablar. Con ellas puede actuar en el mundo real -- leer datos, realizar acciones y devolver resultados a la conversacion.

---

## [01:15] Categorias de herramientas

OpenClaw organiza las herramientas en seis categorias:

**Browser** -- la herramienta de navegador. Permite al agente visitar sitios web, tomar capturas de pantalla, hacer clic en elementos, extraer contenido. En un rato vamos a profundizar en esta.

**Canvas** -- para creacion de contenido visual. Dibujar, generar imagenes.

**Node** -- operaciones a nivel de sistema. Leer y escribir archivos, hacer solicitudes HTTP.

**Cron** -- tareas programadas. Recordatorios, verificaciones periodicas, automatizaciones recurrentes.

**Webhooks** -- endpoints HTTP. Permiten que sistemas externos disparen acciones en tu agente.

Y **Exec** -- ejecucion de codigo. Correr scripts, ejecutar comandos del sistema.

Cada categoria se puede habilitar o deshabilitar independientemente para cada agente. Asi tienes control granular sobre lo que cada agente puede y no puede hacer.

---

## [02:05] Flujo de ejecucion de herramientas

Veamos como decide un agente usar una herramienta.

Cuando el agente recibe una solicitud del usuario, lo primero que hace es evaluar: necesito una herramienta para responder esto? Si la respuesta es no -- por ejemplo, si es una pregunta simple como "que es Python?" -- el agente responde directamente.

Pero si necesita informacion externa o realizar una accion, selecciona la herramienta apropiada. Luego, el sistema verifica si el sandbox esta habilitado. Si lo esta, la herramienta se ejecuta en un entorno aislado. Si no, se ejecuta directamente.

El resultado de la herramienta vuelve al agente, que lo procesa e incorpora en su respuesta final al usuario.

Todo este ciclo sucede de forma transparente -- el usuario solo ve la pregunta y la respuesta. El uso de herramientas es invisible.

---

## [02:50] Herramienta de navegador en detalle

La herramienta de navegador -- o "Browser tool" -- es una de las mas poderosas de OpenClaw.

> **Nota de pronunciacion:** "Browser" se pronuncia "brauser" y significa navegador.

Utiliza Puppeteer para controlar un navegador Chromium sin interfaz grafica -- lo que se conoce como "headless".

> **Nota de pronunciacion:** "Puppeteer" se pronuncia "papetir" y "headless" se pronuncia "jedles", que significa sin cabeza, refiriendose a un navegador sin interfaz visual.

Con esta herramienta, tu agente puede: navegar a cualquier URL, tomar capturas de pantalla, hacer clic en elementos e interactuar con formularios, extraer contenido de paginas, esperar a que cargue contenido dinamico, y manejar multiples pestanas.

Un ejemplo practico: le dices a tu agente "revisa el clima en weather.com". El agente piensa "necesito el browser tool", navega a weather.com, extrae los datos del pronostico, y te responde "actualmente hace 22 grados y esta soleado". Todo automatico.

Y lo importante: como es un navegador real, funciona con sitios pesados en JavaScript, aplicaciones de pagina unica, y paginas que requieren autenticacion.

---

## [03:45] Herramienta Exec

La herramienta Exec permite ejecutar comandos del sistema de forma controlada.

> **Nota de pronunciacion:** "Exec" se pronuncia "exek" y es abreviacion de "execute", ejecutar.

Puedes configurar una lista blanca de comandos permitidos -- por ejemplo, ls, cat, grep, curl, ping, node, python3. Tambien defines un timeout para prevenir procesos que se queden corriendo eternamente. Y opcionalmente habilitas el sandbox para aislamiento.

Un consejo de seguridad muy importante: en produccion, **siempre** restringe los comandos permitidos al minimo necesario. No dejes la lista abierta. Un agente con acceso irrestricto al sistema operativo es un riesgo serio, especialmente si procesa entrada de usuarios no confiables.

---

## [04:20] Que son los skills?

Ahora hablemos de los skills -- que yo traduzco como habilidades.

> **Nota de pronunciacion:** "Skills" se pronuncia "skils" y significa habilidades.

Un skill es un paquete reutilizable que combina un prompt del sistema, configuracion de herramientas, y comportamiento en una sola unidad que cualquier agente puede usar.

Piensa en los skills como "apps" para tu agente. Un skill de busqueda web, por ejemplo, incluye el prompt que le dice al agente "puedes buscar la web usando la herramienta de navegador, siempre cita tus fuentes", la configuracion que habilita la herramienta de browser, y las frases de activacion como "busca", "investiga" o "encuentra en linea".

Los skills son **componibles** -- un agente puede usar multiples skills simultaneamente. Y se pueden combinar para crear flujos de trabajo complejos.

---

## [04:55] Fuentes de skills

Los skills vienen de tres fuentes:

**Incluidos** -- vienen integrados con OpenClaw de fabrica. Cubren casos de uso comunes como resumen, traduccion y busqueda web.

**Workspace** -- son skills personalizados que tu creas en el directorio /skills de tu espacio de trabajo. Totalmente a tu medida.

Y **ClawHub** -- un marketplace comunitario donde puedes explorar, instalar y compartir skills con otros usuarios de OpenClaw.

> **Nota de pronunciacion:** "ClawHub" se pronuncia "clo jab" -- similar a como suena "GitHub" pero con "Claw".

Puedes mezclar skills de las tres fuentes sin problemas.

---

## [05:25] Modos de sandbox

Ahora, la seguridad. El sandbox -- o caja de arena -- es lo que protege tu sistema cuando los agentes ejecutan herramientas.

Hay tres modos:

**Off** -- sin sandbox. Las herramientas se ejecutan directamente en el sistema host. Solo recomendado para desarrollo en entornos de confianza.

**Non-main** -- sandbox solo para agentes no principales. El agente principal se ejecuta con confianza, pero los sub-agentes estan aislados.

**All** -- sandbox para todo. Toda la ejecucion de herramientas esta aislada. Es la configuracion recomendada para produccion.

La implicacion de seguridad es clara: con el sandbox desactivado, un ataque de inyeccion de prompt podria llevar a la ejecucion de codigo arbitrario en tu sistema. Siempre usa "all" en produccion o cuando proceses entrada de usuarios no confiables.

---

## [05:55] Arquitectura del sandbox

El sandbox usa una tecnologia llamada isolated-vm para crear un entorno de ejecucion completamente separado.

> **Nota de pronunciacion:** "Isolated VM" se pronuncia "aisoleitid vi em" y significa maquina virtual aislada.

Lo que el sandbox proporciona: aislamiento de memoria, limites de tiempo de CPU, acceso restringido al sistema de archivos, restricciones de red, y cero acceso al proceso principal.

Lo que el sandbox previene: leer archivos arbitrarios, hacer llamadas de red no autorizadas, acceder a variables de entorno, y crear procesos hijos.

El codigo se ejecuta dentro del sandbox y los resultados se pasan de vuelta al agente por un canal controlado. Es como una habitacion segura donde el codigo puede correr sin poder escapar.

---

## [06:20] Conclusiones clave

Tres cosas para recordar de esta sesion:

**Uno:** las herramientas le dan a tus agentes capacidades del mundo real. Desde navegar la web hasta ejecutar codigo, transforman agentes conversacionales en asistentes verdaderamente utiles.

**Dos:** los skills son paquetes reutilizables. Combinan prompts, herramientas y triggers en "apps" modulares que puedes crear, compartir e instalar desde ClawHub.

**Tres:** el sandbox protege tu sistema. Siempre habilitalo en produccion. Aisla la ejecucion de herramientas y previene acceso no autorizado al sistema host.

---

## [06:40] Cierre / Recursos

Excelente! Eso fue la Sesion 5.

Para profundizar, revisa la documentacion de herramientas en **docs.openclaw.ai/tools**, la documentacion de skills en **docs.openclaw.ai/skills**, y el marketplace de ClawHub en **clawhub.openclaw.ai**.

Y ahora viene la gran final: la Sesion 6, donde vamos a cubrir instalacion, configuracion y seguridad. Vamos a llevar todo lo que aprendimos a la practica -- desde instalar OpenClaw desde cero hasta dejarlo listo para produccion.

Es la sesion donde todo se junta. Nos vemos ahi!

---

*Fin del guion - Sesion 5*
