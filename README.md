# Trabajo Práctico » Insumos Médicos

## Universidad Nacional de Quilmes

### Arquitectura de Software

> Este es el repositorio principal
> para poder centralizar la documentación, justificaciones,
> workflow y todo tipo de información necesaria para
> cumplimentar los requerimientos del trabajo práctico.
> Al mismo tiempo sirve como punto de entrada para
> acceder al resto de los repositorios de forma sencilla.

#### Equipo: Grupo 2

* Leandro Di Lorenzo » [@leandrojdl](https://github.com/leandrojdl)
* Julián Fischetti » [@fischettij](https://github.com/fischettij)

#### Información

* [Enunciado](https://docs.google.com/document/d/1AyV7urbQM0ywcCVH7bCqCrsFQ7miNpRu_-kGEK_vt7A/edit#)
* [Tablero Kanban](https://github.com/orgs/unq-arqsoft-difi/projects/1)
* [Documento de Arquitecturas](Arquitecturas.md)

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

### Etapa 1.1: Registro de Usuario y Login

> Objetivo: desarrollar una funcionalidad mínima que permita realizar un MVP
> sobre las diferentes arquitecturas para poder compararlas.
> Funcionalidad a desarrollar: Carga de usuario.

La idea de esta etapa es poder desarrollar el MVP del producto
mediante dos Arquitecturas diferentes de manera de poder compararlas
y, mediante las fundamentaciones adecuadas, elegir una por sobre la otra
para completar el desarrollo.

En nuestro caso las Arquitecturas planteadas son:

* [Backend API Node][repo-node] + [Frontend React][repo-react]
* [Backend API Kotlin][repo-kotlin] + [Frontend Vue][repo-vue]

Para ver más en detalle el análisis de las arquitecturas se puede consultar el [Documento de Arquitecturas](Arquitecturas.md).

#### Interfaz API

Para lograr que las duplas de Arquitecturas sean intercambiables
planteamos una interfaz de API REST común en ambos _backend_.
De esta forma no quedamos atados a tener que elegir una Arquitectura
completa, dejando la posibilidad de elegir el _backend_ de un "equipo"
y el _frontend_ del otro, ya que los _frontend_ se comunican con el _backend_
mediante la API que estos exponen.

| Method | Path     | JWT | Response | Descripción                                |
|--------|----------|-----|----------|--------------------------------------------|
| `GET`  | `/users` | :ballot_box_with_check:  | `200`    | [Listar usuarios][api-listar-usuarios]     |
| `POST` | `/users` | :x:  | `201`    | [Registrar usuario][api-registrar-usuario] |
| `POST` | `/login` | :x:  | `200`    | [Login de usuario][api-login-de-usuario]   |

Para información detalla consultar la [Documentación de API REST](API-REST.md).

[repo-node]:   <https://github.com/unq-arqsoft-difi/covid-back-node>
[repo-kotlin]: <https://github.com/unq-arqsoft-difi/covid-back-kotlin>
[repo-react]:  <https://github.com/unq-arqsoft-difi/covid-front-react>
[repo-vue]:    <https://github.com/unq-arqsoft-difi/covid-front-vue>
[api-listar-usuarios]:   API-REST.md#listar-usuarios
[api-registrar-usuario]: API-REST.md#registrar-usuario
[api-login-de-usuario]:  API-REST.md#login-de-usuario
