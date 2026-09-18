# Cluster-db

Cluster de base de datos PostgreSQL 16 sobre Docker, con replicacion streaming
(1 primario + 2 secundarios), HAProxy como proxy/balanceador, pgAdmin como
cliente de administracion, y Prometheus + Grafana para monitorizacion.

## Estructura del proyecto

- `docker-compose.yml` - definicion de todos los servicios
- `config/haproxy.cfg` - configuracion del balanceador
- `monitoring/prometheus.yml` - configuracion de scraping de metricas
- `node1/`, `node2/`, `node3/` - documentacion de cada nodo
- `scripts/` - scripts de benchmarking y carga de datos
- `backups/` - respaldos generados durante las pruebas

## Como levantar el cluster

### Opcion A: Portainer (repositorio Git)
1. Subir este repo a GitHub/GitLab.
2. En Portainer: Stacks > Add stack > Repository.
3. Pegar la URL del repo, rama `main`, y en "Compose path" poner `docker-compose.yml`.
4. Deploy the stack.

### Opcion B: linea de comandos
```
docker compose up -d
```

## Usuarios de base de datos

- `administrador` (postgres): superusuario, mantenimiento del motor.
- `replicacion`: usado unicamente para la replicacion streaming entre nodos.
- `aplicacion`: usuario de lectura/escritura para la base `clusterdb`.
- `monitorizacion`: usado por HAProxy (pgsql-check) y por los exporters de Prometheus.

Las contraseñas reales se definen directamente en `docker-compose.yml` (o via `.env`
si se decide externalizarlas) y no se suben a este README.

## Puntos de acceso

- Escrituras: `localhost:5432` (via HAProxy, redirige al primario)
- Lecturas: `localhost:5433` (via HAProxy, balanceo round robin)
- Panel HAProxy: `http://localhost:7000`
- pgAdmin: `http://localhost:8080`
- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000` (dashboard importado: ID 9628)
