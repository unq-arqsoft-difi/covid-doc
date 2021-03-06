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

#### URLs

* Web Page » <https://difi-covid.tk>
* API » <https://api.difi-covid.tk>
* DA Admin » <https://adminer.difi-covid.tk>
* Access Log Monitor » <https://logs.difi-covid.tk>

#### Info » Arq. Soft. II

* [Tablero Kanban](https://github.com/orgs/unq-arqsoft-difi/projects/2)
* Actividades y Entregas:
  1. [Actividad sobre API Backend](API-Changelog.md)
  2. [Checkpoint #1: Integrar sistema de Monitoreo](Checkpoint-1_Monitoreo.md)
  3. [Reporte Final - Monitoreo](Monitoreo.md)
* Repositorios
  - [Backend Node](https://github.com/unq-arqsoft-difi/covid-back-node)
  - [Frontend React](https://github.com/unq-arqsoft-difi/covid-front-react)
  - [API Use Cases](https://github.com/unq-arqsoft-difi/covid-api-use-cases)

#### Info » Arq. Soft. I

> Los repositorios vigentes fueron marcados con el tag `arq1-final-release`
> para indicar la versión correspondiente a trabajo realizado en _Arq. Soft. I_.

* [Enunciado](https://docs.google.com/document/d/1AyV7urbQM0ywcCVH7bCqCrsFQ7miNpRu_-kGEK_vt7A/edit#)
* [Tablero Kanban](https://github.com/orgs/unq-arqsoft-difi/projects/1)
* [Análisis y Comparativa de Arquitecturas](Analisis-Arquitecturas.md)
* [Requerimientos No Funcionales](Requerimientos-No-Funcionales.md)
* [Descripción de Arquitectura](Arquitectura.md)
* [Reflexión Final](Reflexion-Final.md)
* Repositorios
  - [Backend Node](https://github.com/unq-arqsoft-difi/covid-back-node/tree/arq1-final-release)
  - [Frontend React](https://github.com/unq-arqsoft-difi/covid-front-react/tree/arq1-final-release)
  - [Backend Kotlin](https://github.com/unq-arqsoft-difi/covid-back-kotlin) _ARCHIVADO_
  - [Frontend Vue](https://github.com/unq-arqsoft-difi/covid-front-vue) _ARCHIVADO_

## Idea General

> "El objetivo del trabajo práctico es desarrollar un sistema que permita la carga
> y almacenamiento de la información de solicitudes de insumos para combatir la pandemia".

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
* `13/07` Etapa 2.0 » Funcionalidad completa y deploy automático

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

Para ver más en detalle el análisis de las arquitecturas se puede consultar
el [Documento de Arquitecturas](Analisis-Arquitecturas.md).

### Etapa 1.2: Carga de Solicitud

> **Deadline:** 22/06

**Features:**

* Login
* Carga de solicitud de insumos por parte del usuario
* Visualización de solicitudes por parte del usuario (se debe poder ver el estado de cada solicitud)
* Posibilidad de cancelar aquellas que no hayan sido confirmadas

### Etapa 2.0: Funcionalidad Completa y Deploy Automático

> **Deadline:** 13/07

**Features:**

* Registro y Login
* Carga de solicitud de insumos por parte del usuario
* Visualización de solicitudes por parte del usuario (se debe poder ver el estado de cada solicitud)
* Posibilidad de cancelar aquellas que no hayan sido confirmadas
* [Admin] Ver todas las solicitudes realizadas
* [Admin] Posibilidad de aprobar o rechazar solicitudes
* Continuous Integration
* Continuous Deployment

[repo-node]:   <https://github.com/unq-arqsoft-difi/covid-back-node>
[repo-kotlin]: <https://github.com/unq-arqsoft-difi/covid-back-kotlin>
[repo-react]:  <https://github.com/unq-arqsoft-difi/covid-front-react>
[repo-vue]:    <https://github.com/unq-arqsoft-difi/covid-front-vue>
