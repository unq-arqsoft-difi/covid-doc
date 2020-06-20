# Documento de Análisis y Comparativa de Arquitecturas

Este documento comienza detallando las características de cada
stack tecnológico elegido para desarrollar el [MVP] de la aplicación,
para luego poder optar por aquel que consideramos que mejor se ajusta
a las necesidades del producto y del desarrollo.

En nuestro caso se plantean dos _stacks_ de Backend + API REST
([Node][repo-node] y [Kotlin][repo-kotlin])
y dos de Frontend ([React][repo-react] y [Vue][repo-vue]).

Inicialmente se trabajó por "equipos":

* [Back Node][repo-node] + [Front React][repo-react]
* [Back Kotlin][repo-kotlin] + [Front Vue][repo-vue]

pero luego de la etapa de análisis esas combinaciones pueden llegar a cruzarse.
O sea, los análisis de tecnologías se hacen por un lado entre _Node_ y _Kotlin_
y por el otro entre _React_ y _Vue_. Esto es posible por la característica adoptada
de contar con ambos _backend_ que exponen API REST con interfaz común.

## Características de cada tecnología

### Kotlin

> Parte de esta descripción tiene como base el artículo
> [Why you should totally switch to Kotlin] pero también se ven plasmadas
> las experiencias personales del equipo con esta tecnología.

Trabajar con Kotlin requiere trabajar en un ambiente de Java. Es un lenguaje
estáticamente tipado que compila a _bytecode_ de la [JVM] de Java, con lo cual
si bien es necesario manejarse en ese ambiente, plantea una mejora sustancial
en cuanto a legibilidad de código. Es posible -por lo tanto- generar aplicaciones
muy robustas con gran soporte de concurrencia, tolerancia a fallos y gran poder
de procesamiento en cálculos complejos. Todo el poder que brinda Java.

Kotlin no solo tiene inter-operabilidad con Java sino que trae grandes ventajas
a nivel de sintaxis y estructura de código:

* Inferencia de Tipos
* Casteo Inteligente
* Argumentos default y argumentos por nombre
* Estructuras como funciones (if/when)
* Extensión de funciones
* etc...

Como contrapartida está el hecho de que muchas herramientas de Java (si bien
son fácilmente integrables) son difíciles de utilizar en el ambiente Kotlin
por estas diferencias de verbosidad de los lenguajes. Van saliendo nuevas herramientas
que plantean usos _kotlin-style_ pero aún no todas tienen la maduración necesaria.

Enfocándonos solo en el back (porque es posible desarrollar frontend con Kotlin también),
nosotros no quisimos tomar la elección de [Spring] + [Hibernate] (el clásico de Java)
para API y persistencia porque considerábamos que era frameworks demasiado grandes
para el scope de la aplicación.
Fuimos por herramientas más nuevas y creadas para el estilo de kotlin: [Javalin] + [Exposed].
Ambos frameworks cumplen las expectativas y simplifican el desarrollo. A [Exposed]
(desarrollada también por [JetBrains], lo que establece una gran integración con Kotlin)
aún le falta un poco de maduración ya que están en versiones jóvenes: [0.24.1][exposed-version].

### Node

El ambiente de desarrollo de Node consta de un editor de texto y un gestor de paquetes
de Javascript (npm o yarn). Esto le da la flexibilidad al desarrollador de poder
trabajar en el ambiente que se sienta mas cómodo.
Es un lenguaje interpretado y débilmente tipado, esto nos permite poder ejecutar
código sin necesidad de compilarlo. Sin embargo el dolor de cabeza puede venir
en el uso de funciones asincrónicas, que en etapas tempranas del aprendizaje
de la tecnología, puede generar complicaciones difíciles de detectar.
[Node] cuenta con frameworks bastante maduros y con una comunidad grande.
A la hora de seleccionar los frameworks a utilizar, pudimos contar con bastante información
y ejemplos de uso de cada framework.

Para el desarrollo de este [MVP] se seleccionaron los siguientes frameworks principales:

* [Sequelize]: Persistencia. Es un ORM con soporte para persistir en varios tipos de bases de datos. La ventaja de este framework es que podemos alternar el tipo de persistencia dependiendo del ambiente en el que nos encontremos. Por ejemplo en test utilizamos SQLite en memoria y en desarrollo SQLite en un archivo.
* [Express]: Servidor web. Es sencillo de utilizar y nos brinda bastante flexibilidad a la hora de procesar un request.
* [jsonwebtoken]: Librería que nos permite generar y validar JWT.

#### Desventajas detectadas

[Node] es _Single-threaded_. Para suplir este problema, en caso de necesitarlo,
deberíamos levantar varias instancias del servidor y utilizar un _load-balancer_.
Tampoco es eficiente en el cálculo de datos pesados.

### React

> Parte de esta descripción tiene utiliza como fundamento
> el artículo [7-reasons-why-you-should-use-react],
> pero también se ven reflejadas las experiencias
> personales del equipo desarrollando en esta tecnología.

React es un _framework_/_library_ que permite crear interfaces
web utilizando el concepto de Single Page Application (SPA).
React es desarrollado y mantenido por Facebook, y si bien no fue
el primero en plasmar el concepto de SPA, sí se destaca por
ser pionero en la posibilidad de acoplamiento a desarrollos
existentes. [Angular] ya venía trabajando con este concepto pero
funcionaba completamente como framework, con lo cual no era sencillo
(y muchas veces imposible) integrarlo en proyectos pre-existentes.

Como React funciona como _library_, es posible integrarlo en un proyecto
existente y simplemente comenzar a crear partes nuevas con React pero
manteniendo la funcionalidad restante tal como estaba programada.
Esto le permitió a React hacerse paso en la comunidad.

Para destacar a nivel técnico, React tiene una curva de aprendizaje
corta, lo que permite que en el corto plazo cualquier desarrollador
logre tener funcionando una aplicación mínima y con los suficientes
conocimientos para poder seguir indagando. Pero quizás los aspectos
más destacables de React sean Virtual DOM, JSX y los recientes Hooks.

#### Virtual DOM

Al representar aplicaciones web mediante HTML se genera una estructura
de árbol llamada [DOM] (Documento Object Model). Al generar aplicaciones
dinámicas (como SPA), es necesario manipular esos elementos. Para ello
es necesario manipular el [DOM], generando operaciones a nivel
de estructura del árbol.

Estas operaciones son muy costosas para el browser. Lo que hacen los
frameworks que utilizan Virtual DOM es justamente mantener un DOM Virtual
en memoria y eventualmente sincronizar contra el DOM de manera optimizada.
Esto permite mejorar notablemente la _performance_ de las aplicaciones
dinámicas.

#### JSX

JSX es quizás la característica más representativa de React.
Para poder generar SPAs React manipula el DOM a través del VDOM.
Esta manipulación la realiza utilizando métodos que no son muy diferentes
a los clásicos métodos de javascript para manipular el DOM.

La gran diferencia es que React construyó un DSL que abstrae al desarrollador
de esos métodos y permite escribir código "a la html" pero que en realidad
es código javascript.

> En esta página se puede visualizar lo descripto: [What is JSX?][w3school-jsx]

#### Hooks

En el último año React incorporó un nuevo concepto: _Hooks_.

Hasta entonces cada componente interactivo en React requería cumplir
con un [ciclo de vida bien definido][react-state-lifecycle]. Si bien
permite muchas facilidades, con el tiempo los componentes comenzaban
a incrementar su complejidad inclusive aquellos más sencillos. A partir
de ello React incorporó el concepto de [Hooks][react-hooks] que permite
poder clear componentes más concisos sin perder funcionalidad ni dinamismo.

#### Estética

A nivel estética de UI se optó por el uso del framework [Material-UI]
para lograr un diseño acorde a los últimos estándares sin necesidad
de gastar tiempo en mejorar los aspectos estéticos.

### Vue

> Parte del análisis surge de los artículos
> [Reasons Why Vuejs Is Popular][reasons-why-vuejs-is-popular]
> y [VueJS Overview][vuejs-overview]
> pero también de la experiencia del equipo trabajando con
> esta tecnología.

[Vue], al igual que [React], logró re-adaptar el concepto
iniciado por [Angular] para lograr mayor flexibilidad y adaptabilidad
tanto en desarrollos nuevos como en aplicaciones pre-existentes.
[Vue] fue desarrollado originalmente por Evan You, un ex-empleado de Google.

[Vue] y [React] comparten muchísimas similitudes: Virtual DOM, baja
curva de aprendizaje, componentes reactivos, buena comunidad.
Para no volver con ciertas descripciones, vamos a enfocarnos
en las características propias de [Vue] en comparación con [React].

#### Templates vs JSX

La principal diferencia al utilizar [Vue] con respecto a [React]
es el manejo de componentes. En lugar de utilizar [JSX], [Vue]
trabaja con templates HTML y mantiene una separación bien
marcada entre el aspecto visual y la lógica del componente.
Esto permite, a diferencia de [React], lograr una mejor organización
de cada componente y no mezclar lógica de UI con lógica de interacción.

Este concepto no fue introducido por [Vue] sino que fue adaptado
de lo que ya venía haciendo [Angular]. Consiste es escribir código
HTML/XML que luego es pre-procesado para lograr el dinamismo necesario
en las SPA.

#### Data Binding

Si bien el [data-binding] no es un concepto propio de [Vue], sí logra
aprovecharlo de la mejor manera con mucha facilidad para el desarrollador.
Alcanza con definir una propiedad en el componente y agregar el atributo
`v-bind="prop"` en el template para enlazar la información y actualizarla
automáticamente al ser modificada.

#### Event Handling

Esta característica por supuesto tampoco es propia de [Vue] pero
nuevamente permite generar un uso muy simple de ella. Al ser necesario
capturar un evento, y tener esta separación template-lógica, es posible
definir la lógica del evento en javascript puro y en el template
agregar un elemento con el atributo `v-on="method"`.

#### Computed Properties

Otra característica interesante es el hecho de poder mantener una separación
(a nivel lógica del componente) entre métodos dinámicos y valores "fijos".
Hay cierta información que necesita re-calcularse cada vez pero otras no.
Para ello [Vue] permite separar en `methods` y `data`, pudiendo optimizar
la _reactividad_ de los componentes y generar mejor performance. A la vez
que permite una mejor organización para el desarrollador

#### Estética

A nivel estética de UI, al igual que en [React], se optó por el uso
de un framework. En este caso se optó por [Vuetify], también un framework
basado en [Material Design][material-design] que permite lograr un diseño
acorde a los últimos estándares sin necesidad
de gastar tiempo en mejorar los aspectos estéticos.

## Elección y justificación

### Backend

> Parte de la justificación en la decisión surge de lo artículos
> [Difference Between Node.js and Java Performance] y
> [Why Node.js beats Java and .Net for Web, mobile, and IoT apps].
> También se tuvo en cuenta las experiencias personales del equipo.

Tanto Node/Javascript como Kotlin/JVM son arquitecturas sólidas, muy utilizadas,
con grandes comunidades y buena performance. A nivel técnico es muy difícil
lograr sobreponer a una por sobre la otra de una forma clara.

Además, es importante tener en cuenta el contexto sobre el que se está desarrollando.
Esta es una aplicación que si bien tiene un impacto a nivel nacional, lo que
requiere robustez en su arquitectura, es también un proyecto que está atado
a una emergencia sanitaria, con lo cual es imperioso que el proyecto
salga a producción lo más rápido posible. Este fue el factor decisivo
a la hora de tomar una decisión.

Ambos tenemos experiencia utilizando Node y Kotlin, pero nos
sentimos muchos más cómodos trabajando sobre Node. En el día a día, cuando
surgen problemas en el desarrollo, nos es más sencillo resolver esos problemas
cuando estamos trabajando en un ambiente de Node/Javascript que en uno de Kotlin/JVM.

El otro punto significativo (aunque no tan decisivo) fue la comparación de los
frameworks de persistencia. Tanto [Sequelized] como [Exposed] son grandes frameworks,
robustos y simples de utilizar. Pero [Exposed] no está tan maduro como [Sequelized]
y eso, en un proyecto con el tiempo apremiante, puede llegar a ser un problema.

Con respecto a cuestiones de performance, tal como se plantea en el artículo
[Why Node.js beats Java and .Net for Web, mobile, and IoT apps],
el hecho de que [Node] no tenga concurrencia y maneje las peticiones
mediante _nonblocking I/O_, permite que la aplicación sea menos compleja
dado que no existe la necesidad de manejar concurrencia y administrar
zonas de exclusión mutua, lo que suele ser problemático y difícil de rastrear
en caso de errores.

Si bien podría pensarse que una solución concurrente es
más eficiente (y puede serlo en el contexto adecuado),
eso no es tan cierto en soluciones web donde mayor cuello de botella se da justamente
en las peticiones HTTP sobre Internet.
Este "delay" en las peticiones permite que una arquitectura _event-driven_
con _nonblocking I/O_ no pierda performance significativa en comparación con
una arquitectura concurrente ya que el tiempo que se "pierde" entre
_request_ y _response_ permite a la aplicación procesar la información
con el tiempo suficiente como para lograr atender la petición siguiente
sin que es "tiempo de espera" sea crucial.

También es importante notar que la aplicación funciona con una base de datos,
lo cual implica consultas sucesivas al motor. Y sea tanto una aplicación
concurrente como no, para las peticiones de escritura, el _lock_ de proceso
se va a dar a nivel Base de Datos. No así para lectura, pero sí para escritura.

Además también hay que tener en cuenta que esta aplicación no va a trabajar
con cómputos complejos ni números tan grandes o tan chicos que requieran
mayor y mejor procesamiento. Se trata de una aplicación de solicitudes
básicas, donde es aceptable "pagar" unos pocos segundos de retraso
en pos de la seguridad de la respuesta.

Con todo esto y sumado a que las características del equipo de desarrollo
están mejor enfocadas a [Node] es que nos inclinamos por utilizar el
[API Backend Node][repo-node]. La "pérdida" de performance que podría
darse entre un sistema concurrente como uno JVM-based y uno sin concurrencia como Node
se "gana" en un código mucho más simple desde Node y menos propenso a errores.
También el hecho de no necesitar cómputos complejos (donde sin duda
un sistema JVM-based es mucho más eficiente) pone un escalón por encima a Node.

Lo primordial en una aplicación de tiempo crítico como esta es poder
salir a producción lo antes posible y con la menor complejidad.
En este punto es donde Node gana, por su simplicidad de arquitectura
(manteniendo una performance muy aceptable) y por los conocimientos
del equipo de desarrollo.

### Frontend

> Si bien el artículo
> [Vue vs React: Which is the Best JavaScript Framework in 2020?][vue-vs-react-battle-javascript]
> describe una buena comparativa y lo utilizamos como base,
> el grueso de la decisión fue de carácter personal dentro de los miembros del equipo.

A modo de comparativa de comunidades dejamos análisis de búsquedas entre
[Vue] y [React]. Aunque no fueron fundamentales a la hora de tomar una decisión,
creemos que es interesante poder visualizar cómo se mueven las comunidades:

* **[Stack Overflow Trends][stack-overflow-react-vs-vue]**
  - Poco más del 4% de las consultas son sobre [React]
  - Mientras que el 1.5% corresponden a preguntas sobre [Vue]
* **[Google Trends][google-trends-react-vs-vue]**
  - En esta métrica de búsqueda vemos que desde el 2017 en adelante React superó a Vue.
* **[NPM trends][npm-trends]**
  - El resultado es similar al de Stack overflow.

#### Decisión

Tanto [Vue] como [React] son muy similares en casi todos los aspectos.
No podemos asegurar con certeza que uno sobrepase a otro en cuestiones
técnicas ni en ventajas de desarrollo. Con lo cual no podemos
utilizar estas características para definir una elección.

De manera que el haber optado por uno sobre la otra se debe meramente
a gustos personales y comodidad del equipo trabajando con la tecnología.

Si bien ambos conocíamos ambas tecnologías, nos sentimos un poco más
cómodos trabajando con [React]. Y al igual que en la elección del _backend_,
priorizamos este hecho por el contexto y la urgencia de la aplicación.

## Stack Elegidos

* [Backend Node](https://github.com/unq-arqsoft-difi/covid-back-node)
* [Frontend React](https://github.com/unq-arqsoft-difi/covid-front-react)

[7-reasons-why-you-should-use-react]: <https://stories.jotform.com/7-reasons-why-you-should-use-react-ad420c634247>
[Angular]: <https://angular.io/>
[data-binding]: <https://en.wikipedia.org/wiki/Language_binding>
[Difference Between Node.js and Java Performance]: <https://www.educba.com/node-js-vs-java-performance/>
[DOM]: <https://es.wikipedia.org/wiki/Document_Object_Model>
[exposed-version]: <https://bintray.com/kotlin/exposed/exposed-core/0.24.1>
[Exposed]: <https://github.com/JetBrains/Exposed>
[Express]: <https://expressjs.com/>
[google-trends-react-vs-vue]: <https://trends.google.com/trends/explore?cat=31&date=2015-01-01%202020-05-24&q=Vue%20%2B%20Vue.js%20%2B%20vue,React,Angular>
[Hibernate]: <https://hibernate.org/>
[Javalin]: <https://javalin.io/>
[JetBrains]: <https://www.jetbrains.com/>
[jsonwebtoken]: <https://www.npmjs.com/package/jsonwebtoken>
[JVM]: <https://en.wikipedia.org/wiki/Java_virtual_machine>
[material-design]: <https://material.io/design>
[Material-UI]: <https://material-ui.com/>
[MVP]: <https://en.wikipedia.org/wiki/Minimum_viable_product>
[Node]: <https://nodejs.org/en/>
[npm-trends]: <https://www.npmtrends.com/react-vs-vue-vs-@angular/core?ref=hackernoon.com>
[React]: <https://reactjs.org/>
[react-hooks]: <https://reactjs.org/docs/hooks-intro.html>
[react-state-lifecycle]: <https://reactjs.org/docs/state-and-lifecycle.html>
[reasons-why-vuejs-is-popular]: <https://www.monterail.com/blog/reasons-why-vuejs-is-popular>
[repo-kotlin]: <https://github.com/unq-arqsoft-difi/covid-back-kotlin>
[repo-node]: <https://github.com/unq-arqsoft-difi/covid-back-node>
[repo-react]: <https://github.com/unq-arqsoft-difi/covid-front-react>
[repo-vue]: <https://github.com/unq-arqsoft-difi/covid-front-vue>
[Sequelize]: <https://sequelize.org/>
[Sequelized]: <https://github.com/sequelize/sequelize/>
[Spring]: <https://spring.io/>
[stack-overflow-react-vs-vue]: <https://insights.stackoverflow.com/trends?tags=vue.js%2Cvuejs2%2Creactjs%2Cangular>
[vue-vs-react-battle-javascript]: <https://deliciousbrains.com/vue-vs-react-battle-javascript>
[Vue]: <https://vuejs.org>
[vuejs-overview]: <https://www.tutorialspoint.com/vuejs/vuejs_overview.htm>
[Vuetify]: <https://vuetifyjs.com/>
[w3school-jsx]: <https://www.w3schools.com/react/react_jsx.asp>
[Why Node.js beats Java and .Net for Web, mobile, and IoT apps]: <https://www.infoworld.com/article/2975233/why-node-js-beats-java-net-for-web-mobile-iot-apps.html>
[Why you should totally switch to Kotlin]: <https://medium.com/@magnus.chatt/why-you-should-totally-switch-to-kotlin-c7bbde9e10d5>
