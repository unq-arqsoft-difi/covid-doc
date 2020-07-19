# CORS

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

## Situación en nuestra aplicación

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

**Fuente:** [Mozilla][cors-mozilla]

[cors-mozilla]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
