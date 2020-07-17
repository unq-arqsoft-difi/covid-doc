# CORS

Cors es un protocolo o estrategia requerida por los navegadores web para evitar un tipo de ataque de denegación de servicios.

Esto consiste en, en cada pedido ajax que se quiere ejecutar en el browser, antes este envia a esa misa url un request de metodo OPTIONS y un header 'Origin' el cual indica cual es el dominio desde el cual se esta haciendo la petición.

El servidor valida la existencia de este request y segun la estrategia configurada, la indica al browser que puede continuar con las piteciones. Actualmente en la etada incial, de los primeros deployment, tenemos configurado CORS con un * esto quiere decir que una web desde cualquier dominio puede hacer uso del api.

La idea es condigurar CORS para que solo sea posible invocar request desde nuestro dominio de la aplicación. La configuración no tiene complicaciones, solo es necesario indicar el o los dominios desde donde queremos que se ejecuten los pedidos.

En conclusión, CORS no puede ser "apagado" dado que es algo que maneja el navegador. Si desde el back dejamos de validar esto, ningun pedido puede ser ejecutado.

## Ejemplo de ataque sin la existencia de CORS:
Supongamos que nuestro back soporta 100 request por segundo y tenemos una ocurrencia promedio de 20 request por segundo. En este escenario estamos ok.

Ahora, suponiendo que existe una pagina a la que acceden miles de personas por minuto y exploan una vulnerabilidad de dicho sitio, podrían agregar codigo malicioso en esta pagina para que cada vez que cargue, se envie un request a nuestro back. En este caso pasariamos a superer la cantidad de request soportados, afectando la performance de la aplicación.

**Fuente:** [Mozilla][cors-mozilla]

[cors-mozilla]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS