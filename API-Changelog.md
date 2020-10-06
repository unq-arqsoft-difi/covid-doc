# API Changelog

> Este documento busca documentar los cambios en la API entre la
> versión final de Arq. Soft. I y las nuevas modificaciones generadas
> a partir de los intercambios en Arq. Soft II.
> Para conceptos de buenas prácticas _RESTful_ no basamos en articulo
> [Best practices for REST API design](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/).

## 2020-07-20 » Arq.1 Final Release

La versión final de la API desarrollada para la materia
_Arquitectura de Software I_ buscó ser _RESTful_ pero quedaron algunas cuestiones
por mejorar. A continuación se detallan los endpoints de la API tal como
estaban al finalizar _Arquitectura de Software I_.

| Method   | Path                                  | Token | Token-Admin | Descripción                         |
|----------|---------------------------------------|-------|-------------|-------------------------------------|
| `POST`   | `/login`                              | ✔️    | ❌          | Login de Usuario                     |
| `POST`   | `/users`                              | ✔️    | ❌          | Registrar Nuevo Usuario              |
| `POST`   | `/request-supplies`                   | ✔️    | ❌          | Registrar Nueva Solicitud            |
| `GET`    | `/request-supplies`                   | ✔️    | ❌          | Ver Solicitudes                      |
| `GET`    | `/request-supplies/:id`               | ✔️    | ❌          | Ver Solicitud                        |
| `DELETE` | `/request-supplies/:id`               | ✔️    | ❌          | Cancelar Solicitud                   |
| `GET`    | `/support/areas`                      | ❌    | ❌         | Listado de Áreas                     |
| `GET`    | `/support/institutions`               | ❌    | ❌         | Listado de Hospitales                |
| `GET`    | `/support/provinces`                  | ❌    | ❌         | Listado de Provincias                |
| `GET`    | `/support/provinces/:id`              | ❌    | ❌         | Provincia                            |
| `GET`    | `/support/provinces/:id/towns`        | ❌    | ❌         | Provincia con Listado de Localidades |
| `GET`    | `/support/supplies`                   | ❌    | ❌         | Listado de Suministros               |
| `GET`    | `/support/providers`                  | ❌    | ❌         | Listado de Proveedores               |
| `GET`    | `/admin/request-supplies`             | ✔️    | ✔️          | Listado de todas las Solicitudes     |
| `GET`    | `/admin/request-supplies/:id`         | ✔️    | ✔️          | Obtener Solicitud                    |
| `PUT`    | `/admin/request-supplies/:id/approve` | ✔️    | ✔️          | Aprobar Solicitud                    |
| `PUT`    | `/admin/request-supplies/:id/reject`  | ✔️    | ✔️          | Rechazar Solicitud                   |

Se puede observar que hay endpoints que no se cumple con todas las prácticas _RESTful_.
Es por ello que se buscó mejorar este aspecto. Se identificaron las siguientes situaciones:

- Los _endpoints_ con prefijo `admin/` pueden renombrarse para no necesitar el prefijo.
- Los _endpoints_ de cambio de estado (`/accept` y `/reject`) deberían:
  * No ser verbos.
  * Que la acción sea `PATCH` con `body` en lugar de `PUT`.
- El _endpoint_ `POST /login` debería reemplazarse con `POST /session` para hacer referencia
  explícita al objeto sobre el que referencia el _endpoint_ y que no sea una acción.
- El _endpoint_ con prefijo `/support/provinces/:id/towns` no debería contener el `/towns`.
  Para esa información se deberían utilizar _query params_.

## 2020-09-20 » Arq.2

A partir de las situaciones detectadas se generaron cambios en la API de manera de
acercarnos más a las buenas prácticas _RESTful_. Se detallan a continuación los endpoints
resultantes a partir de las modificaciones, tal cómo actualmente funciona la API en
su versión [v2](https://github.com/unq-arqsoft-difi/covid-back-node/releases/tag/v2.0.0).

| Method   | Path                     | JWT | Body | Descripción                                                  |
|----------|--------------------------|-----|------|--------------------------------------------------------------|
| `POST`   | `/session`               | ✔️  | ✔️    | Genera sesión de usuario a partir de las credenciales        |
| `POST`   | `/users`                 | ✔️  | ✔️    | Registra un nuevo usuario                                    |
| `GET`    | `/request-supplies`      | ✔️  | -    | Retorna solicitudes de usuario (si es admin, todas)          |
| `POST`   | `/request-supplies`      | ✔️  | ✔️    | Registra nueva solicitud                                     |
| `GET`    | `/request-supplies/:id`  | ✔️  | -    | Retorna solicitud (si no es admin tiene que ser del usuario) |
| `PATCH`  | `/request-supplies/:id`  | ✔️  | ✔️    | Cambia el estado de la solicitud (a rechazada o aceptada)    |
| `DELETE` | `/request-supplies/:id`  | ✔️  | -    | Cancela Solicitud                                            |
| `GET`    | `/support/areas`         | -   | -    | Listado de Áreas                                             |
| `GET`    | `/support/institutions`  | -   | -    | Listado de Hospitales                                        |
| `GET`    | `/support/provinces`     | -   | -    | Listado de Provincias                                        |
| `GET`    | `/support/provinces/:id` | -   | -    | Provincia (usando `?include=towns` incluye localidades)      |
| `GET`    | `/support/supplies`      | -   | -    | Listado de Suministros                                       |
| `GET`    | `/support/providers`     | -   | -    | Listado de Proveedores                                       |
