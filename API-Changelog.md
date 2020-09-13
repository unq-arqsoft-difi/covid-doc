# API Changelog

> Este documento busca documentar los cambios en la API entre la
> versión final de Arq. Soft. I y las nuevas modificaciones generadas
> a partir de los intercambios en Arq. Soft II.
> Para conceptos de buenas prácticas _RESTful_ no basamos en articulo
> [Best practices for REST API design](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/).

## 2020-07-20 » Arq.1 Final Release

La versión final de la API desarrollada para la materia
_Arquitectura de Software I_ buscó ser _RESTful_ pero quedaron algunas cuestiones
por mejorar. [Ver la documentación de la API al finalizar la cursada](https://github.com/unq-arqsoft-difi/covid-doc/tree/arq1-final-release#interfaz-api).

Se detalla un punteo de situaciones a mejorar:

- Los _endpoints_ con prefijo `admin/` podrían renombrarse para no necesitar el prefijo.
- Los _endpoints_ de cambio de estado (`/accept` y `/reject`) deberían:
  * Ser sustantivos en lugar de verbos.
  * Que la acción sea `PATCH` con `body` en lugar de `PUT`.
- El _endpoint_ `POST /login` debería reemplazarse con `POST /session` para hacer referencia
  explícita al objeto sobre el que referencia el _endpoint_ y que no sea una acción.
