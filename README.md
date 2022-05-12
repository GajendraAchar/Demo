# BISKIT app

Read [Architectural Design Documentation](https://docs.google.com/document/d/1NJq__1h5fz82z9gzAO1uYamIftrzG8yPlqm3ac-56yU/edit#heading=h.ib4c49p5rk8d)

## Applications

### Backend app

Develop in python using [Django](https://www.djangoproject.com/).

See more in [backend/README](backend/README.md).

### Frontend app

Develop in [React](https://reactjs.org/) + [RxDB](https://rxdb.info/).

See more in [frontend/README](frontend/README.md).

### Mobile app

Develop in [React-Native](https://reactnative.dev/) + [RxDB](https://rxdb.info/).

See more in [mobile/README](mobile/README.md).

## Pre-requisites

No local pre-requisites should be needed apart from:

- [docker](https://docs.docker.com/engine/installation/)
- [docker-compose](https://docs.docker.com/compose/)
- [openssl](https://www.openssl.org/) (generates local secrets)

The local setup uses **docker-compose** to spin up all necessary services.
Make sure you have it installed and can connect to the **docker daemon**.

The `docker-compose.yml` file describes the setup of the containers.

Execute the following to prepare the frontend environment

```bash
make prepare
```

This will generate secrets (`.env` file) for the local instances and download the third party containers.

Include this entry in your `/etc/hosts` or `C:\Windows\System32\Drivers\etc\hosts` file:

```text
127.0.0.1     biskit-api.local   # "BACKEND_URL" entry in ".env" file
127.0.0.1     biskit.local       # "FRONTEND_URL" entry in ".env" file
```

## Containers and services

The list of the containers:

| Container         | Description                                             |
|-------------------|---------------------------------------------------------|
| frontend          | [React](https://reactjs.org/) app                       |
| backend           | [Django](https://www.djangoproject.com/) web framework  |
| db                | [PostgreSQL](https://www.postgresql.org/) database      |
| redis             | [Redis](https://redis.io/) server                       |
| nginx             | [NGINX](https://nginx.org/) server                      |
| smtp              | [SMTP](https://github.com/namshi/docker-smtp) server    |

All of the container definitions for development can be found in the `docker-compose.yml`.

## Run

### Build the apps

Run in project directory:

```bash
make build
```

This will build the backend&frontend app containers.

Open your browser and access the `FRONTEND_URL` entry in `.env` file.

The default credentials are defined here.

- admin user: `admin`
- password: `password` (check `BACKEND_ADMIN_PASSWORD` entry in `.env` file).

### Start the apps

Run in project directory:

```bash
make serve
```

This will start the applications behind NGINX along with all dependencies.

The frontend app should be reachable at `FRONTEND_URL` value, `http://biskit.local`.
The backend app should be reachable at `BACKEND_URL` value, `http://biskit-api.local`.

## Start the apps locally in production mode

1. Build production containers:

    ```sh
    make build-prod
    ```

2. Generate secrets:

    ```sh
    ./_scripts/secrets.sh
    ```

3. Change secret values (`.env` file):

    - Frontend app URL settings:
        - `FRONTEND_URL`
        - `FRONTEND_DOMAIN`

    - Backend app URL settings:
        - `BACKEND_URL`
        - `BACKEND_DOMAIN`

    - E-Mail server settings:
        - `EMAIL_HOST`
        - …

4. Run production container:

    ```sh
    make serve-prod
    ```

## Run commands in the containers

The pattern to run a command is always

```bash
docker-compose run --rm [--no-deps] <container-name> <entrypoint-command> <...args>
```

If there is no interaction with any other container then include the option `--no-deps`.

See more in [docker-compose run](https://docs.docker.com/compose/reference/run).

## Documentation

The project documentation is available in [Google drive](https://drive.google.com/drive/folders/1unTYAP3dpFeqvO4xsVev5tPH45tkYQBu).

## Deployments

- New **commits** in the `develop` branch trigger deployments in **dev**
  - Backend: `biskit-backend-dev.eha.im`
  - Frontend: `biskit-frontend-dev.eha.im`
  - Mobile: will create the `alpha` release and push it to the GC dev bucket.

- New **commits** in the `staging` branch trigger deployments in **stage**
  - Backend: `biskit-backend-stage.eha.im`
  - Frontend: `biskit-frontend-stage.eha.im`
  - Mobile: will create the `beta` release and push it to the GC stage bucket.

- New **commits** in the `main` branch trigger deployments in **production**
  - Backend: `biskit-backend.eha.im`
  - Frontend: `biskit-frontend.eha.im`
  - Mobile: will create the `production` release and push it to the GC production bucket.

## LICENSE

> **Copyright 2022 eHealthAfrica**

[Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0)

Licensed under the Apache License, Version 2.0 (the **“License”**);
you may not use this file except in compliance with the **License**.

You may obtain a copy of the **License** at
<http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed
under the **License** is distributed on an **“AS IS”** BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

See the **License** for the specific language governing permissions and
limitations under the **License**.

## Credit

Brought to you by [eHealth Africa](http://ehealthafrica.org/)
— *good tech for hard places*.
