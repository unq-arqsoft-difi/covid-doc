# Requerimientos No Funcionales

## Logging

> Parte de las configuraciones y definiciones adoptadas como estrategias
> de Logging se basaron en el artículo [How To Use Winston to Log Node.js Applications].
> También se incorporaron ideas y experiencias del equipo en el uso de estas tecnologías.

Para cumplir con este requerimiento se va a adoptar una estrategia de Logging
que consta de dos partes:

* Logs de acceso
* Logs de aplicación

**Levels:**

error: 0,
  warn: 1,
  info: 2,
  http: 3,
  verbose: 4,
  debug: 5,
  silly: 6

### Access Logging

Por un lado nos interesa poder resguardar la información de acceso a la aplicación,
sean accesos correctos o errores. El trasfondo de esta estrategia es poder generar
una base de información para poder comprender cómo, cuando y dónde los usuarios
utilizan la aplicación. Con esa información es posible generar análisis de datos
con el fin de establecer un circuito de mejora constante.

Para esta estrategia vamos a utilizar la _library_ [Morgan], la cual permite
acoplarse a la API como un middleware, permitiendo establecer el nivel
de logging a registrar como así también el formato y el lugar de resguardo.

**TODO: Explicar cual estrategia se adoptó para morgan.**

### Application Logging

Con respecto a los logs de aplicación vamos a utilizar la _library_ [Winston].
Este package es de los más extendidos y utilizados en aplicaciones Node.
Es altamente configurable, permitiendo no solo establecer _levels_
de logging sino también diferentes maneras de resguardo (llamados _transports_)
que pueden ser configurados dinámicamente según el _environment_ y las necesidades.

Los _transports_ con medios de loggings auto-configurables. Se pueden definir
_transports_ de archivo, de consola, de base de datos, etc. Toman por defecto
la configuración global pero pueden sobre-escribirse con configuraciones particulares.

**TODO: Explicar cual estrategia se adoptó para winston.**



[How To Use Winston to Log Node.js Applications]: <https://www.digitalocean.com/community/tutorials/how-to-use-winston-to-log-node-js-applications>
[Morgan]: <https://github.com/expressjs/morgan>
[Winston]: <https://github.com/winstonjs/winston>
