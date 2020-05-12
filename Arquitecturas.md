# Documento de Arquitecturas

Este documento comenzará detallando las características (pros y contras) de cada
stack de tecnologías elegidas para desarrollar el MVP de la aplicación,
para luego poder optar por aquellas que consideremos que mejor se ajustan
a las necesidades del producto y del desarrollo.

En nuestro caso se plantean dos _stacks_ de Backend + API REST ([Node][repo-node] y [Kotlin][repo-kotlin])
y dos de Frontend ([React][repo-react] y [Vue][repo-vue].
Inicialmente se trabajará por equipos:

* [Back Node][repo-node] + [Front React][front-react]
* [Back Kotlin][repo-kotlin] + [Front Vue][front-vue]

pero luego de la etapa de análisis esas combinaciones pueden llegar a cruzarse.
O sea, los análisis de tecnologías se harán, por un lado entre _node_ y _kotlin_
y por el otro entre _react_ y _vue_. Esto es posible por la característica adoptada
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

### React

### Vue

## Elección y justificación

### Backend

### Frontend

[repo-node]: <https://github.com/unq-arqsoft-difi/covid-back-node>
[repo-kotlin]: <https://github.com/unq-arqsoft-difi/covid-back-kotlin>
[repo-react]: <https://github.com/unq-arqsoft-difi/covid-front-react>
[repo-vue]: <https://github.com/unq-arqsoft-difi/covid-front-vue>
[JVM]: <https://en.wikipedia.org/wiki/Java_virtual_machine>
[Spring]: <https://spring.io/>
[Hibernate]: <https://hibernate.org/>
[Javalin]: <https://javalin.io/>
[Exposed]: <https://github.com/JetBrains/Exposed>
[JetBrains]: <https://www.jetbrains.com/>
[exposed-version]: <https://bintray.com/kotlin/exposed/exposed-core/0.24.1>
[Why you should totally switch to Kotlin]: <https://medium.com/@magnus.chatt/why-you-should-totally-switch-to-kotlin-c7bbde9e10d5>