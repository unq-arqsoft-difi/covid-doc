# Trabajo Práctico » Insumos Médicos

## Universidad Nacional de Quilmes

### Arquitectura de Software

> Este es el repositorio principal
> para poder centralizar la documentación, justificaciones,
> workflow y todo tipo de información necesaria para
> cumplimentar los requerimientos del trabajo práctico.
> Al mismo tiempo sirve como punto de entrada para
> acceder al resto de los repositorios de forma sencilla.

#### Integrantes

* Leandro Di Lorenzo » [@leandrojdl](https://github.com/leandrojdl)
* Julián Fischetti » [@fischettij](https://github.com/fischettij)

#### Enunciado

* [Arq1 - 2020s1 - Insumos Médicos](https://docs.google.com/document/d/1AyV7urbQM0ywcCVH7bCqCrsFQ7miNpRu_-kGEK_vt7A/edit#)

#### Repositorios

* [Backend Node](https://github.com/unq-arqsoft-difi/covid-back-node)
* [Backend Kotlin](https://github.com/unq-arqsoft-difi/covid-back-kotlin)
* [Frontend React](https://github.com/unq-arqsoft-difi/covid-front-react)
* [Frontend Vue](https://github.com/unq-arqsoft-difi/covid-front-vue)

## Idea General

> "El objetivo del trabajo práctico es desarrollar un sistema que permita la carga y almacenamiento de la información de solicitudes de insumos para combatir la pandemia".

Se establecen dos _stack tecnológicos_ para ser comparados
en una primera etapa. En ambos casos se plantea la
estructura _backend_ - _frontend_ donde _backend_ incluye
lógica de dominio + API REST; y _frontend_ una App [SPA](https://en.wikipedia.org/wiki/Single-page\_application)
que se comunica con el _backend_ mediante la API que este expone.

Se plantean -inicialmente- los equipos **Kotlin + Vue** y **Node + React**,
aunque por la naturaleza de la estructura elegida los equipos pueden ser
intercambiados luego de la etapa de análisis y comparación.

## Desarrollo por etapas

### Etapa 1

La idea de esta etapa es poder desarrollar el MVP del producto
mediante dos Arquitecturas diferentes de manera de poder compararlas
y, mediante las fundamentaciones adecuadas, elegir una por sobre la otra
para completar el desarrollo.

En nuestro caso las Arquitecturas planteadas son:

* [Backend API Node](https://github.com/unq-arqsoft-difi/covid-back-node) +
  [Frontend React](https://github.com/unq-arqsoft-difi/covid-front-react)
* [Backend API Kotlin](https://github.com/unq-arqsoft-difi/covid-back-kotlin) +
  [Frontend Vue](https://github.com/unq-arqsoft-difi/covid-front-vue)

#### Etapa 1.1: Registro de Usuario

> Objetivo: desarrollar una funcionalidad mínima que permita realizar un MVP
> sobre las diferentes arquitecturas para poder compararlas.
> Funcionalidad a desarrollar: Carga de usuario.

##### Interfaz API

Para lograr que las duplas de Arquitecturas sean intercambiables
planteamos una interfaz de API REST común en ambos _backend_.
De esta forma no quedamos atados a tener que elegir una Arquitectura
completa, dejando la posibilidad de elegir el _backend_ de un "equipo"
y el _frontend_ del otro, ya que los _frontend_ se comunican con el _backend_
mediante la API que estos exponen.

###### Registro de usuario

**Request:**

```json
POST /registry
Accept: application/json
body: {
  "firstName": "Jon",
  "lastName": "Snow",
  "email": "jon.snow@winterfell.com",
  "phone": "+54 11 4444-5555",
  "entity": "Hospital Alemán",
  "job": "Enfermero",
  "place": "CABA",
  "pass": "1234"
}
```

**Responses:**

`201 Created`

Body:

```json
{
  "created": true
}
```

`400 Bad Request`

Si hay datos erróneos o email ya registrado.

Body:

```json
{
    "created": false,
    "errors": [
        "E-Mail address already exists",
        "Job is required"
    ]
}
```

###### Login

La autenticación se realizará mediante mail y contraseña y el manejo de sessión
mediante un token otorgado al momento de autenticarse.

El token tendrá una validez de 2 días. Pasado ese tiempo es necesario volver a pedirlo.
**TODO: Justificar**.

**Request:**

```json
POST /login
body: {
  "email": "jon.snow@winterfell.com",
  "pass": "1234"
}
```

**Response:**

`200 OK`

Body:

```json
{
  "token": "123456789"
}
```

`400 Bad request`

Body:

```json
{
  "token": false,
  "errors": [
    "Invalid email or password"
  ]
}
```

##### Secure Endpoint

**Request:**

```json
GET /auth-place
Headers: "Authorization: Bearer 123456789"
```

**Response:**

`200 OK` (O lo que retorne el endpoint particular)

`401 Unauthorized`
