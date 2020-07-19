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
