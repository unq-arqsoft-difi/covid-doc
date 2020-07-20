# Reflexión Final

Por último, les pedimos hacer el siguiente análisis:

Habiendo agregado algunos features más de manera completa, usando
la tecnología que hace unos meses decidieron que era la correcta:

> **¿Encontraron nuevos beneficios o nuevos riesgos sobre la tecnología elegida?**

Con respeto al back, creemos que resultó bastante pragmática la forma de generar nuevas
funcionalidades luego de la prueba de Concepto. Node con Express permite mucha
flexibilidad y realmente no encontramos obstáculos en ese aspecto, todo lo contrario,
tuvimos buenos beneficios, sobre todo con el ORM Sequelize, el cual es muy cómodo de utilizar.

Del lado del front, nosotros decidimos seguir con React y lo utilizamos con su "nuevo sabor"
a través de Hooks. Por supuesto que esto mejora notablemente con respecto al "anterior sabor"
de clases con lifecycle, pero inclusive con los hooks notamos que a medida que los componentes
iban creciendo en complejidad, dicha complejidad terminaba impactando un poco en el código.
Es necesario ser súper prolijo y tener buen manejo y capacidad de extraer componentes en
"pedazos" más chicos y reutilizables, caso contrario el código se vuelve bastante engorroso.
Un caso particular que notamos de complejidad en React es el manejo de state en conjunción con
el localStorage. Fue necesario darle una vuelta de tuerca porque de forma nativa no se
maneja del todo bien.

> **¿Se encontraron en una situación en la cual hayan "re-evaluado" la decisión**
> **y en ese contexto/situación hubieran elegido el stack tecnológico descartado?**
> **(el famoso: "si tan solo hubiera elegido el otro stack")**

Con respecto al backend, seguimos creyendo que Node ofrece las suficientes ventajas sobre
Kotlin (por lo detallado en el documento original) para que mantengamos nuestra posición.
No nos encontramos en alguna situación en la que hayamos "deseado" estar en Kotlin.
Nos resultó sumamente productiva la elección de Node.

Pasando al front, nos encontramos sobre el final del desarrollo que la organización de los
componentes, y los componentes en sí, no terminaron quedando tan "cómodos" como lo hubiésemos
querido. En ciertos momentos notamos que Vue te permite una mejor organización, pero también
es cierto que es muy dependientes de la comodidad y el tiempo de uso sobre la tecnología.
Es cierto que Vue te permite mantener una mejor separación de lógica y visual dentro del
componente, pero también es cierto que es una cuestión de gustos. Con los componentes simples
(al inicio, sobre todo), React nos resultó mucho más pragmático. Al crecer en complejidad,
a veces creíamos que en Vue los hubiésemos podido mantener más organizados. De todas formas,
como dijimos, es una cuestión de gustos sobre cómo se prefiere tener organizado cada
componente. Quizás lo más significativo fue la elección del framework de diseño (Material-UI),
que nos trajo algunos dolores de cabeza, pero no fue algo que haya entrado en discusión al
momento de decidir los stacks. Para concluir, si bien tuvimos esos "pequeños" momentos,
estamos conformes con ambas elecciones y nos llevamos mucho aprendizaje del proceso.
