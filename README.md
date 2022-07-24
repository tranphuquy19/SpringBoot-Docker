# Spring boot - Docker Multi-Stage build

- [What is a multi-stage build?](https://vsupalov.com/docker-multi-stage-advantages/)
- [Official documentation](https://spring.io/guides/topicals/spring-boot-docker/)

_Note: To reduce build time please enable DOCKER_BUILDKIT=1_

```bash
export DOCKER_BUILDKIT=1 # enable Docker buildkit (for Linux)
```

## Usage

### Using docker compose

1. Build app image

```bash
docker-compose -f docker-compose.prod.yml build demo
```

2. Start app

```bash
docker-compose -f docker-compose.prod.yml up -d
# This command will start only postgres and demo app containers
```

If you want to start pgadmin too, you can use the following command:

```bash
docker-compose -f docker-compose.prod.yml --profile debug up -d
# This command will start all containers with debug profile [postgres, demo] + pgadmin4
```

3. Stop app

```bash
docker-compose -f docker-compose.prod.yml stop
```

4. Destroy app

```bash
docker-compose -f docker-compose.prod.yml down -v --remove-orphans
# This will remove all containers and volumes (lost data)
```

5. Start postgres db and pgadmin4 for development

Notice: Please modify the connection string in application.properties file (from `jdbc:postgresql://postgres:5432/demo` -> `jdbc:postgresql://localhost:5432/demo`).

```bash
docker-compose -f docker-compose.dev.yml up -d
# This command will start postgres and pgadmin4 with the ports exposed on host machine
```

### Using docker CLI

1. Build image

```bash
docker build -t demo:latest .
```

2. Build builder image

```bash
docker build --target builder -t demo:builder .
```

3. Run image

```bash
docker run -p 8080:8080 demo:latest
```