# Checkpoint 1

## Monitoreo

Para la elección del sistema de monitoreo analizamos las alternativas
propuestas por la cátedra pero en la mayoría de los casos teníamos
la limitación de ser aplicaciones pagas con _free trial_ de solo 14 días.
Esos nos impendía poner tener implementaciones duraderas durante la cursada.

En un primer momento analizamos e intentamos utilizar _Elastic search_,
pero conversando con los docentes nos propusieron en cambio analizar
la alternativa de [Prometheus](https://prometheus.io/) con
[Grafana](https://grafana.com/), y esta fue la alternativa que
decidimos utilizar.

Además de lo descripto, esta solución nos daba como ventaja que podía adaptarse
bastante bien con nuestra arquitectura _dockerizada_.

> Parte de la implementación se basó en el artículo
> [Build A Monitoring Dashboard by Prometheus + Grafana](https://medium.com/htc-research-engineering-blog/build-a-monitoring-dashboard-by-prometheus-grafana-741a7d949ec2)

Entonces, en el [repositorio de casos de uso](https://github.com/unq-arqsoft-difi/covid-api-use-cases)
armamos sumamos al docker-compose que utilizamos para correr los casos, las configuraciones
de Promehteus con Grafana. Esto nos permitió no solo levantar un ambiente donde correr
los casos de forma independiente sino también monitorizarlo.

Previamente agregamos a la aplicación de API una _library_ encargada de recolectar la información
para prometheus: [api-express-exporter](https://github.com/teamzerolabs/api-express-exporter).

Esta _library_ agrega el middleware:

```js
app.use(apiExpressExporter({ host: '0.0.0.0', port: 9991 }));
```

escuchando y recolectando la información, exponiendo el endpoint `/metrics`
para que Prometheus lo consuma.

La configuración del _docker-compose_ quedó de esta manera:

```yaml
version: '3.7'
services:
  api:
    image: unqdifi/covid-back-node:v2
    restart: unless-stopped
    ports:
      - '9001:9001'
      - '9991:9991'
    environment:
      NODE_ENV: production
      LOG_LEVEL: error
      SERVER_PORT: 9001
      ACCESS_TOKEN_SECRET: 12345
      DATABASE_URL: "postgres://covid:54321@db:5432/covid"
    volumes:
      - ./logs/:/opt/app/logs/
    depends_on:
      - db
      - recreate

  recreate:
    build: .
    command: ["./wait-for-it.sh", "db:5432", "--", "/opt/app/recreate-db.sh"]
    environment:
      NODE_ENV: production
      DATABASE_URL: "postgres://covid:54321@db:5432/covid"
    depends_on:
      - db

  db:
    image: postgres:12
    environment:
      POSTGRES_USER: covid
      POSTGRES_PASSWORD: "54321"
    restart: unless-stopped
    volumes:
      - ./db/data:/var/lib/postgresql/data
  
  prometheus:
    image: prom/prometheus:v2.19.1
    ports:
      - '9090:9090/tcp'
    volumes:
      - prometheus-data:/prometheus/
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana:7.0.3
    ports:
      - '3000:3000/tcp'
    volumes:
      - grafana-data:/var/lib/grafana

volumes:
  prometheus-data:
  grafana-data:
```

Y el archivo de configuración de prometheus, de esta manera:

```yaml
global:
  scrape_interval:     90s # Set the scrape interval to every 90 seconds. Default is every 1 minute.
  evaluation_interval: 90s # Evaluate rules every 90 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'api'

rule_files:

scrape_configs:
  # Use DNS Service discovery
  ## https://github.com/teamzerolabs/api-express-exporter
  - job_name: api
    dns_sd_configs:
      - { names: [ api ], type: A, port: 9991 }
    metrics_path: /metrics
```

Entonces al levantar los container (`docker-compose up`), quedan expuestos los servicios:

- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`
