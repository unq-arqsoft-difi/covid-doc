# Challenge Arquitectural: Publicación de Información

## Enunciado

### Situación

El Gobierno quiere conocer los eventos de compra de insumos de manera periódica
(período variable, suficientemente corto como para tener visibilidad del
progreso de los insumos). Esto como medida de planificación y
seguimiento del volumen de transacciones de pedidos.

### Restricción

El Gobierno no quiere publicar servicio alguno, por lo que no es viable
enviar estímulos a sistemas del Gobierno. El Gobierno informa que la
integración sería a través de una aplicación (no lo va a consumir un humano).

### Requerimiento

Pensar y diseñar (al menos dos) alternativas de componentes dentro
de la arquitectura elegida (no necesariamente implementarlas),
donde permitan consumir la información requerida por el Gobierno.  

_Tips_: Pensar en preguntas como:

* ¿Cuál es la estrategia de integración elegida?
* ¿Cómo vamos a controlar qué sistemas del Gobierno acceden?
* ¿Cuál es la interfaz y/o protocolo de integración?  

### Formato de entrega

Adjuntar el informe en formato PDF y en el campo de texto libre
adjuntar link a la fuente online.

## Resolución

Dado que el Gobierno no quiere exponer servicios desde su lado,
y que el consumo será automatizado a través de una aplicación,
la única alternativa que entendemos viable es exponer de forma
constante la información desde nuestra aplicación.
Lo que que sí puede plantearse como alternativa es si la información
se generá _on-demand_ o se pre-calculará de antemano.

La generación on-demand permite contar con datos más fiables,
pero si el volumen es muy grande puede relentizar el consumo.

Por el contrario, si la información se pre-calcula, el consumo
de la misma es más eficaz pero la información puede estar desactualizada.

### Opción 1: Exposición On-Demand

Se genera el endpoint `GET /insumos` en API-Backend que retorne
un listado de insumos comprados.

> Dado que es información crítica, el endpoint será protegido
> por un token de aplicación previamente autorizado. Solo podrá
> existir un único token válido, con lo cual, si el Gobierno
> necesita cambiar de aplicación o se perdió el token, al generar
> uno nuevo se invalida el anterior.

El _request_ consume la base de datos y retorna la información
de la compra de insumos.

Dado que el _request_ será incremental en el transcurso del tiempo,
se agregar los parámetros opcionales de rango de fecha, para
limitar la información. O sea: `GET /insumos?from=:dateFrom&to=:dateTo`.

Si `to` no existe, se toma la fecha actual.

Si `from` no existe, se la define como 30 días antes de `to`.

Ejemplos válidos:

* `GET /insumos`
  - **from**: 2020-06-01
  - **to**: 2020-06-30 (día actual)
* `GET /insumos?from=2020-01-01`
  - **from**: 2020-01-01
  - **to**: 2020-06-30 (día actual)
* `GET /insumos?to=2020-01-31`
  - **from**: 2020-01-01
  - **to**: 2020-01-31
* `GET /insumos?from=2020-02-01&to=2020-02-28`
  - **from**: 2020-02-01
  - **to**: 2020-02-28

### Opción 2: Información Pre-calculada

Se genera el endpoint `GET /insumos/:fecha.json` en API-Backend que retorne
un listado de insumos comprados para el día `:fecha`.

Además se genera el endpoint `GET /insumos` en API-Backend que retorne
un listado de todos los archivos `:fecha.json` disponibles hasta el momento.

> Dado que es información crítica, ambos endpoints serán protegidos
> por un token de aplicación previamente autorizado. Solo podrá
> existir un único token válido, con lo cual, si el Gobierno
> necesita cambiar de aplicación o se perdió el token, al generar
> uno nuevo se invalida el anterior.

Al finalizar el día se corre un proceso que genera un _dump_
de la información para el día que acaba de finalizar y se guarda
en disco en formato `json` a fecha de información.

Por ejemplo, el `2020-05-23` a las `03:00 AM` se corrió el proceso
para resguardar las solicitudes del día `2020-05-22`. Al finalizar
se generó el archivo `2020-05-22.json` con la información correspondiente.

Si el proceso falla no se genera el archivo y se envía una alarma
a los responsables de la información.

## Notas sobre el Esquema de Autenticación

La aplicación utiliza Json Web Token ([JWT]) como sistema de autenticación
y manejo de acceso a recursos. Particularmente utiliza la _library_ [node-jsonwebtoken].

Al momento del _login_ se genera un token para ese usuario. Ese token
permite, además de validar la sesión, acceder a los recursos que se exponen
desde el _backend_. Como se puede observar en los [endpoints](README#interfaz-api)
publicados por la API, los endpoints requiere validación de token para
retornar las respuestas. Quedan exceptuados los endpoints de _support_,
_registro_ y _login_. Support porque es información de consulta para
utilizar como soporte en los formularios. Login y Register por cuestiones obvias.

Además del token de usuario, existe otro tipo de token exclusivo para usuarios
administradores. Estos token permiten administrar las solicitudes.

No es posible, a nivel de usuario de front o consultas directas a la API
hacerse de un token de admin dado que la interfaz de API no expone en ningún
momento algún campo que permita hacerse pasar por admin. Los usuarios
administradores son creados directamente por nosotros directamente por base.

Por supuesto que ninguna aplicación está exenta de hacking, pero para nuestra
aplicación, y en esta instancia, creemos que las posibilidades de hacking están más
ligadas a la ingeniería social que a vulnerabilidades de la aplicación.

### CORS

> Fuente: [Cross-Origin Resource Sharing (CORS)][cors-mozilla]

Cors es un protocolo o estrategia requerida por los navegadores web
para evitar un tipo de ataque de denegación de servicios.
Es un mecanismo que utiliza cabeceras HTTP adicionales para permitir
que un _user agent_ (el navegador) obtenga permiso para acceder a recursos
desde un servidor pero en un origen (dominio) distinto al que pertenece.

Entonces, en cada pedido AJAX que se necesita ejecutar, el navegador
previamente envía a esa misma URL un request `OPTIONS` y en el campo
`Origin` pone el valor del dominio de la Web (ej, google.com).
Como este valor difiere del valor desde donde efectivamente se hace
el pedido (ej, mi casa: 200.156.067.042).

Sin habilitación de CORS estas peticiones son tomadas como maliciosas y se bloquean.
Con CORS habilitado en el servidor se valida la existencia de estos
requests de dominios cruzados (_cross domain_) y según la estrategia configurada
le indica al navegador si puede continuar con las peticiones.

La especificación de CORS sugiere que los exploradores "verifiquen" la solicitud
solicitando métodos soportados desde el servidor con un método de solicitud
`HTTP OPTIONS` y luego, con la "aprobación" del servidor, enviar la verdadera
solicitud con el método de solicitud HTTP verdadero.

#### Situación en nuestra aplicación

Actualmente en la etapa inicial, con los primeros deployment,
optamos por tener una configuración CORS sin restricciones. O sea,
cualquier web, desde cualquier dominio podría, en principio, hacer
requests hacia la API. Esto no representa mayores problemas
dado que la aplicación no expone ni permite manipulación de
información sensible con los _token_ de usuario. La información
sensible (administración de peticiones) solo es posible manipularla
con token de administrador.

La idea es configurar CORS para que solo sea posible invocar request desde nuestro
dominio de la aplicación. La configuración no tiene complicaciones, solo es necesario
indicar él o los dominios desde donde queremos que se ejecuten los pedidos.

En conclusión, CORS no puede ser "apagado" dado que es algo que maneja el navegador
para todos los casos de aplicaciones _client rendering_, como es nuestro caso.
Si desde el back dejamos de validar CORS, ningún pedido puede ser ejecutado.

En los casos de aplicaciones _server rendering_, aún utilizando una API,
sí es posible deshabilitar CORS dados que los pedidos se harían internamente.

[JWT]: https://jwt.io
[node-jsonwebtoken]: https://github.com/auth0/node-jsonwebtoken
[cors-mozilla]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
