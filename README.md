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
* [Análisis de Arquitecturas](Arquitecturas.md)
* [Requerimientos No Funcionales](Requerimientos-No-Funcionales.md)

#### Repositorios

* [Backend Node](https://github.com/unq-arqsoft-difi/covid-back-node)
* [Frontend React](https://github.com/unq-arqsoft-difi/covid-front-react)
* [Backend Kotlin](https://github.com/unq-arqsoft-difi/covid-back-kotlin) _DEPRECADO_
* [Frontend Vue](https://github.com/unq-arqsoft-difi/covid-front-vue) _DEPRECADO_

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

## Metodología de Trabajo

En la primer etapa, al estar trabajando sobre dos tipos de _stacks tecnológicos_
para poder tomar una decisión en cuanto a qué tecnologías adoptar, lo que hicimos
fue separarnos las tareas y que cada uno trabaje en un stack distintos.
Julián estuvo trabajando sobre Node y React y Leandro sobre Kotlin y Vue.

Pero para que todo el equipo esté al tanto de los desarrollos de ambos MVP,
adoptamos una metodología de trabajo en _branchs_ y _code reviews_ cruzados
en los _pull requests_. O sea, Si Julián trabaja en una funcionalidad en Node,
no lo hace sobre _master_ sino que abre un nuevo _branch_. Cuando termina
arma un _pull request_ pero no lo _mergea_. El encargado de revisar la funcionalidad
y aceptar el _pull request_ es en este caso Leandro. Lo mismo sucede al revés.
Si Leandro arma una funcionalidad en Vue, el encargado de llevar eso a _master_
es Julián previa revisión del código.

Esto nos permitió que ambos podamos trabajar en cada uno de los _stacks_ pero
a la vez estando al tanto todo el tiempo del desarrollo del otro _stack_.
Por supuesto que esto fue complementado con una comunicación continua.

## Desarrollo por etapas

### Fechas importantes

* `25/05` Etapa 1.1 » Elección de Arquitectura
* `22/06` Etapa 1.2 » Funcionalidad completa del lado del usuario (sin admin)
* `20/07` Etapa 2.0 » Funcionalidad completa y deploy automático

### Etapa 1.1: Registro de Usuario y Login

> **Deadline:** 25/05

**Objetivo:**

Desarrollar una funcionalidad mínima que permita realizar un MVP
sobre las diferentes arquitecturas para poder compararlas.
Funcionalidad a desarrollar: Carga de usuario.

La idea de esta etapa es poder desarrollar el MVP del producto
mediante dos Arquitecturas diferentes de manera de poder compararlas
y, mediante las fundamentaciones adecuadas, elegir una por sobre la otra
para completar el desarrollo.

En nuestro caso las Arquitecturas planteadas son:

* [Backend API Node][repo-node] + [Frontend React][repo-react]
* [Backend API Kotlin][repo-kotlin] + [Frontend Vue][repo-vue]

Para ver más en detalle el análisis de las arquitecturas se puede consultar el [Documento de Arquitecturas](Arquitecturas.md).

### Etapa 1.2: Carga de Solicitud

> **Deadline:** 22/06

**Features:**

* Login
* Carga de solicitud de insumos por parte del usuario
* Visualización de solicitudes por parte del usuario (se debe poder ver el estado de cada solicitud)
* Posibilidad de cancelar aquellas que no hayan sido confirmadas

#### Interfaz API

Para lograr que las duplas de Arquitecturas sean intercambiables
planteamos una interfaz de API REST común en ambos _backend_.
De esta forma no quedamos atados a tener que elegir una Arquitectura
completa, dejando la posibilidad de elegir el _backend_ de un "equipo"
y el _frontend_ del otro, ya que los _frontend_ se comunican con el _backend_
mediante la API que estos exponen.

| Method | Path     | JWT | Response | Descripción                                |
|--------|----------|-----|----------|--------------------------------------------|
| `GET`  | `/users` | :heavy_check_mark:  | `200`    | [Listar usuarios][api-listar-usuarios]     |
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
