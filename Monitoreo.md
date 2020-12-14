# Monitoreo

Para generar monitoreo con tests de carga se utilizó un sistema dockerizado mediante
docker-compose donde se establecen los siguiente servicios:

* API (con exportación de métricas de requests)
* DB Postgres
* Prometheus como recolector de métricas
* Grafana como visualizador
* Node-Exporter como sistema de exportación de métricas del sistema

Se puede visualizar el
[docker-compose utilizado](https://github.com/unq-arqsoft-difi/covid-api-use-cases/blob/v1.0.0/docker-compose.yml)
en el repositorio.

## Test de carga realizados

Se utilizó [Artillery](https://artillery.io/) como herramienta de generación de tests.

Se generaron dos suites de tests:

### Suite 1

Esta primer suite es simplemente una serie de requests de endpoints de _support_.
La utilizad es generar información básica y simple sobre datos estable que igual requieran
acceso a la base de datos.

```yaml
phases:
  - duration: 60
    arrivalRate: 5
    bame: Warm up
scenarios:
  - name: "Get support resources"
    flow:
      - get:
          url: "/support/areas"
      - get:
          url: "/support/institutions"
```

### Suite 2

Por el contrario, esta segunda suite genera:

* Registros de usuarios
* Login de los usuarios
* Pedidos de Insumos por parte de los usuarios
* Login del admin
* Consulta del admin sobre los pedidos obtenidos

La intención es no sólo generar los pedidos sino verificar el comportamiento
de visualización de pedidos a medida que van creciendo. De todas formas,
para que la aplicación no explote se generó paginado de visualización de pedidos.

```yaml
phases:
  - duration: 60
    arrivalRate: 2
    name: Warm up
  - duration: 120
    arrivalRate: 2
    rampTo: 15
    name: Ramp up load
  - duration: 420
    arrivalRate: 10
    name: Sustained load
  - duration: 420
    arrivalRate: 15
    name: Heavy
  - duration: 420
    arrivalRate: 12
    rampTo: 15
    name: Crash
scenarios:
  - post:
    url: "/users"
  - post:
    url: "/session"
  - post:
    url: "/request-supplies"
  - post:
    url: "/session"
  - get:
    url: "/request-supplies?status=Pending"
```

## Resultados 1: Sin limitación del sistema

Las pruebas se corrieron en un ambiente local (dockerizado, como se explicó al inicio)
con las siguientes características:

* CPU: Intel i5 con 4 cores
* Memoria: 16gb
* Disco: 500gb SSD

### Gráfica de resultados

#### Mediciones de Respuestas

![Reporte de Responses - Corrida 1](img/Reporte-Responses-corrida1.png)

#### Mediciones de Uso de CPU y Memoria

![Reporte de CPU y Memoria - Corrida 1](img/Reporte-CPU-Memory-corrida1.png)
