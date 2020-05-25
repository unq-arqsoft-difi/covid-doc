# Documento de Arquitecturas

Este documento comenzará detallando las características (pros y contras) de cada
stack de tecnologías elegidas para desarrollar el MVP de la aplicación,
para luego poder optar por aquellas que consideremos que mejor se ajustan
a las necesidades del producto y del desarrollo.

En nuestro caso se plantean dos _stacks_ de Backend + API REST ([Node][repo-node] y [Kotlin][repo-kotlin])
y dos de Frontend ([React][repo-react] y [Vue][repo-vue].
Inicialmente se trabajará por equipos:

* [Back Node][repo-node] + [Front React][repo-react]
* [Back Kotlin][repo-kotlin] + [Front Vue][repo-vue]

pero luego de la etapa de análisis esas combinaciones pueden llegar a cruzarse.
O sea, los análisis de tecnologías se harán, por un lado entre _Node_ y _Kotlin_
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
que plantean un usos _kotlin-style_ pero aún no todas tienen la maduración necesaria.

Enfocándonos solo en el back (porque es posible desarrollar frontend con Kotlin también),
nosotros no quisimos tomar la elección de [Spring] + [Hibernate] (el clásico de Java)
para API y persistencia porque considerábamos que era frameworks demasiado grandes
para el scope de la aplicación.
Fuimos por herramientas más nuevas y creadas para el estilo de kotlin: [Javalin] + [Exposed].
Ambos frameworks cumplen las expectativas y simplifican el desarrollo. A [Exposed]
(desarrollada también por [JetBrains], lo que establece un gran acoplamiento con Kotlin)
aún le falta un poco de maduración ya que están en versiones jóvenes: [0.24.1][exposed-version].

### Node

El ambiente de desarrollo de Node consta de un editor de texto y un gestor de paquetes de javascript (npm o yarn). Esto le da la flexibilidad al desarrollador de poder trabajar en el ambiente que se sienta mas cómodo.
Es un lenguaje interpretado y débilmente tipado, esto nos permite poder ejecutar el código sin necesidad de compilarlo. Sin embargo el dolor de cabeza puede venir en el uso de funciones asincrónicas, que en etapas tempranas del aprendizaje de la tecnología, puede dar dolores de cabeza.
NodeJs cuenta con frameworks bastante maduros y con una comunidad grande. A la hora de seleccionar los frameworks a utilizar, pudimos contar con bastante información y ejemplos de uso de cada framework.

Para el desarrollo de este MVP se seleccionaron los siguientes frameworks principales:

* [Sequelize]: Persistencia. Es un ORM con soporte para persistir en varios tipos de bases de datos. La ventaja de este framework es que podemos alternar el tipo de persistencia dependiendo del ambiente en el que nos encontremos. Por ejemplo en test utilizamos SQLite en memoria y en desarrollo SQLite en un archivo.
* [Express]: Servidor web. Es sencillo de utilizar y nos brinda bastante flexibilidad a la hora de procesar un request.
* [jsonwebtoken]: Librería que nos permite generar y validar JWT.

**Desventajas detectadas:**

Node.js es _Single-threaded_. Para suplir este problema, en caso de necesitarlo, deberíamos levantar varias instancias del servidor y utilizar un balancer.
No es eficiente en el cálculo de datos pesados.

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
frameworks que utilizando VDOM es justamente mantener un DOM Virtual
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

_TODO_

## Elección y justificación

### Backend

> Parte de la justificación en la decisión surge del artículo
> [Difference Between Node.js and Java Performance]
> pero fundamentalmente se debe a experiencias personales del equipo.

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

En definitiva: nos inclinamos por utilizar el [API Backend Node][repo-node]
fundamentalmente por las características del equipo de desarrollo,
lo cual, creemos, permite agilizar la puesta en producción,
que en este contexto es un punto crucial.

### Frontend

_TODO_
* **Stack overflow:** Poco más del 4% de las consultas son sobre react, mientras que el 1.5% corresponden a preguntas sobre Vue. [stack-overflow-react-vs-vue]. Pero si vemos otro dato están más parejos [stack-overflow-react-vs-vue-2].
* **Google Trends:** En esta métrica de búsqueda vemos que desde el 2017 en adelante React superó a Vue. [google-trends-react-vs-vue].
* **NPM trends:** El resultado es similar al de Stack overflow. [npm-trends].

---

[repo-node]: <https://github.com/unq-arqsoft-difi/covid-back-node>
[repo-kotlin]: <https://github.com/unq-arqsoft-difi/covid-back-kotlin>
[repo-react]: <https://github.com/unq-arqsoft-difi/covid-front-react>
[repo-vue]: <https://github.com/unq-arqsoft-difi/covid-front-vue>
[JVM]: <https://en.wikipedia.org/wiki/Java_virtual_machine>
[Spring]: <https://spring.io/>
[Hibernate]: <https://hibernate.org/>
[Javalin]: <https://javalin.io/>
[Exposed]: <https://github.com/JetBrains/Exposed>
[Sequelized]: <https://github.com/sequelize/sequelize/>
[JetBrains]: <https://www.jetbrains.com/>
[Angular]: <https://angular.io/>
[exposed-version]: <https://bintray.com/kotlin/exposed/exposed-core/0.24.1>
[Why you should totally switch to Kotlin]: <https://medium.com/@magnus.chatt/why-you-should-totally-switch-to-kotlin-c7bbde9e10d5>
[Difference Between Node.js and Java Performance]: <https://www.educba.com/node-js-vs-java-performance/>
[Sequelize]: <https://sequelize.org/>
[Express]: <https://expressjs.com/>
[jsonwebtoken]: <https://www.npmjs.com/package/jsonwebtoken>
[stack-overflow-react-vs-vue]: <https://insights.stackoverflow.com/trends?tags=vue.js%2Cvuejs2%2Creactjs%2Cangular>
[stack-overflow-react-vs-vue-2]: <https://insights.stackoverflow.com/survey/2019#technology-_-most-loved-dreaded-and-wanted-web-frameworks>
[google-trends-react-vs-vue]: <https://trends.google.com/trends/explore?cat=31&date=2015-01-01%202020-05-24&q=Vue%20%2B%20Vue.js%20%2B%20vue,React,Angular>
[npm-trends]: <https://www.npmtrends.com/react-vs-vue-vs-@angular/core?ref=hackernoon.com>
[7-reasons-why-you-should-use-react]: <https://stories.jotform.com/7-reasons-why-you-should-use-react-ad420c634247>
[w3school-jsx]: <https://www.w3schools.com/react/react_jsx.asp>
[react-state-lifecycle]: <https://reactjs.org/docs/state-and-lifecycle.html>
[react-hooks]: <https://reactjs.org/docs/hooks-intro.html>
[Material-UI]: <https://material-ui.com/>
[DOM]: <https://es.wikipedia.org/wiki/Document_Object_Model>
