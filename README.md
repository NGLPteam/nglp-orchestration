## Development Environment

This project uses docker and docker-compose to setup a development environment for the NGLP platforms. You'll need to install a [current version of docker](https://docs.docker.com/docker-for-mac/install/).

Start things up with `docker-compose up`.

The following containers will be started:

- nglp-platform-es01 (elastic search node)
- nglp-platform-es02 (elastic search node)
- nglp-platform-es03 (elastic search node)
- nglp-platform-kib01 (kibana)
- nglp-platform-beanstalkd (beanstalkd)
- nglp-platform-postgres (postgres)
- nglp-platform-keycloak (keycloak)
- nglp-platform-reverse-proxy (nginx)
- nglp-platform-frontend (next.js)

And we're exposing the following services through an Nginx reverse-proxy which is routing top-level paths to specific containers.

The containers persist data in various directories underneath the `volumes` directory in this repository. To wipe out existing data and start over, simply remove the `volumes` directory, run `docker compose down` and bring the containers backup with `docker compose up`.

- frontend: [http://localhost:8080](http://localhost:8080)
- kibana: [http://localhost:8080/kibana](http://localhost:8080/kibana)
- keycloak: [http://localhost:8080/auth](http://localhost:8080/auth)

Postgres is also exposed on port 15432 for now, mainly for debugging purposes.

### Service Notes

#### Keycloak

Keycloak is available at [http://localhost:8080/auth](http://localhost:8080/auth). When the container is created, an admin user will be created with the username and password specified in `KEYCLOAK_ADMIN_USER` and `KEYCLOAK_ADMIN_PASSWORD`. If these environment variables are not set, default username is `admin` and the default password is `Pa55w0rd`.

#### Frontend

The frontend container's image is built from the `main` branch of NGLPteam/nglp-wdp-frontend by default. If, however, you would like to build it from your local source, create a .env file in the root of this orchestration project and set the `FRONTEND_BUILD_CONTEXT` environment variable to a relative path to the local frontend repository, such as `../frontend`. If you build from local source, you should be able to make changes in the app and see them reflected in the browser without needing to rebuild the image.

### Docker Compose Notes

#### Networking

For now, this docker-compose file creates three networks and attempts to segment services to the appropriate network: backend, frontend, and elastic. This architecture should be considered fairly WIP, as we're still sorting out exactly what services will be involved in this platform.

#### Rebuilding Images

Rebuild a specific image with the following command (replace `frontend` with the container name):

```
docker-compose up --build --force-recreate --no-deps frontend
```

### Under Development

This repo is being actively developed and is still very much a work in progress. More soon!