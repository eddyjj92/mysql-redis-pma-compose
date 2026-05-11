# MySQL + Redis + phpMyAdmin con Docker Compose

## Requisitos

- Docker + Docker Compose
- Copiar `.env.example` a `.env` y ajustar credenciales

```bash
cp .env.example .env
```

## Levantar servicios

```bash
docker compose up -d
```

## Accesos

| Servicio    | URL                           |
|-------------|-------------------------------|
| phpMyAdmin  | http://localhost:${PMA_HOST_PORT:-8080} |
| MySQL       | localhost:${MYSQL_PORT:-3306}          |
| Redis       | localhost:${REDIS_PORT:-6379}          |

## Credenciales por defecto (ver .env)

- Root: `rootpassword`
- Usuario app: `appuser` / `apppassword`
- Base de datos: `appdb`

## Parar servicios

```bash
docker compose down
```

## Eliminar datos persistentes

```bash
docker compose down -v
```

## Red compartida (cross-compose)

Conectar contenedores de otros `compose.yml` a MySQL/Redis:

```bash
# Crear red externa (solo primera vez)
docker network create shared_net
```

En el otro proyecto, agregar al `compose.yml`:

```yaml
networks:
  shared_net:
    external: true

services:
  tu-servicio:
    networks:
      - shared_net
```

Los servicios `mysql_db`, `phpmyadmin`, `redis_db` ya estan en `shared_net`.

Conectarse por nombre de contenedor:

| Servicio      | Hostname       |
|---------------|----------------|
| MySQL         | `mysql_db`     |
| Redis         | `redis_db`     |
