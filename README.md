## Development Environment

This project uses docker and docker-compose to setup a development environment for the NGLP platforms. You'll need to install a [current version of docker](https://docs.docker.com/docker-for-mac/install/).

The docker-compose file in this root expects to find the frontend application at ../frontend. To satisfy this requirement, clone `NGLPteam/nglp-wdp-frontend` at the same level as this repository. We'll remove this dependency as we develop this further.

Start things up with `docker-compose up`.

The following containers will be started:

- es01 (elastic search node)
- es02 (elastic search node)
- es03 (elastic search node)
- nglp-platform-kib01 (kibana) <-- Still WIP!
- nglp-platform-beanstalkd (beanstalkd)
- nglp-platform-postgres (postgres)
- nglp-platform-keycloak (keycloak)
- nglp-platform-reverse-proxy (nginx)
- nglp-platform-frontend (next.js)

And we're exposing the following services through an Nginx reverse-proxy which is routing top-level paths to specific containers.

frontend: [http://localhost:8080](http://localhost:8080)
kibana: [http://localhost:8080/kibana](http://localhost:8080/kibana)
keycloak: [http://localhost:8080/auth](http://localhost:8080/auth)

Postgres is also exposed on port 15432 for now, mainly for debugging purposes.

More soon!