# Collector Configuration

- Set the Kafka broker addresses in the `collector/.env` file:
  - `KAFKA_BROKER_1`, `KAFKA_BROKER_2`, `KAFKA_BROKER_3`
- Specify the `HOST_NAME` where it's installed (e.g., `java-app-server`, `python-app-server`).
- Set the query-service server address in the `collector/otel-collector-opamp-config.yaml` file.

---

# Signoz Backend Configuration (`signoz-backend-setup/.env`)

- Set the `HOST_NAME` where it's installed (e.g., `staging-signoz`, `prod-signoz`).
- Specify the path where you want to persist data:
  - `SIGNOZ_DATA_PATH`
- Add the ClickHouse server address where it’s installed:
  - `CLICKHOUSE_DB_HOST` (e.g., `clickhouse` if using Docker)
  - `CLICKHOUSE_DB_PORT`
- Set the Kafka broker addresses:
  - `KAFKA_BROKER_1`, `KAFKA_BROKER_2`, `KAFKA_BROKER_3`
- Set the query-service server address in `otel-collector-opamp-config.yaml`.

---

# Example `.env` Files

## `collector/.env`

```plaintext
HOST_NAME=staging-java-api

KAFKA_BROKER_1=192.168.1.45:29094
KAFKA_BROKER_2=192.168.1.45:29095
KAFKA_BROKER_3=192.168.1.45:29096
```


## `signoz-backend-setup/.env`

```plaintext
HOST_NAME=staging-java-api
ALERTMANAGER_TAG=0.23.7
DOCKER_TAG=0.55.0
OTELCOL_TAG=0.102.10

HOST_NAME="staging-signoz-host"
DOCKER_MULTI_NODE_CLUSTER=false

SIGNOZ_DATA_PATH="./data"

CLICKHOUSE_DB_HOST=clickhouse
CLICKHOUSE_DB_PORT=9000

KAFKA_BROKER_1=192.168.1.45:29094
KAFKA_BROKER_2=192.168.1.45:29095
KAFKA_BROKER_3=192.168.1.45:29096
```

## Running the collector
```plaintext
docker compose -f collector/docker-compose-otel-signoz.yaml -p otel-colletor-signoz up -d
```

## Running the Signoz Backend
```plaintext
docker compose -f docker-compose.yaml up -d
```


## Resolving Kafka Failures
Sometimes when restarting the backend, Kafka may fail to start. To resolve this, use the following steps:


1. Take the services down:
```
docker compose -f docker-compose.yaml down -v
```

2. Remove old Kafka data:
```
sudo rm -rf data/kafka1-data/* data/kafka2-data/* data/kafka3-data/* data/zookeeper_kafka/*
```

3. Start the services again:
```
docker compose -f docker-compose.yaml up -d
```