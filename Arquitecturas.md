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
> [Why you should totally switch to Kotlin] pero también se ve plasmada
> la experiencia personal con esta tecnología.

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

_TODO_

### React

_TODO_

### Vue

_TODO_

## Elección y justificación

### Backend

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
[exposed-version]: <https://bintray.com/kotlin/exposed/exposed-core/0.24.1>
[Why you should totally switch to Kotlin]: <https://medium.com/@magnus.chatt/why-you-should-totally-switch-to-kotlin-c7bbde9e10d5>