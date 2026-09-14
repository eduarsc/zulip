# Docker Compose development environment

This Compose configuration runs the Zulip development server from the current
checkout. It uses the same image and provisioning flow as Zulip's supported
Vagrant-with-Docker environment.

Set `HOST_UID` to your numeric user ID so that generated files remain writable
on the host, then build and start the environment:

```console
$ HOST_UID=$(id -u) docker compose up --build
```

The first start provisions all system, Python, and JavaScript dependencies and
builds the development databases, so it can take several minutes. When it is
ready, open <http://localhost:9991/>. Source changes on the host are reloaded by
the development server.

Stop the server with `Ctrl+C`. A later `docker compose up` reuses the provisioned
container. `docker compose down` removes that container, so the next start needs
to provision the environment again. Files generated in the checkout remain
available.

The following optional environment variables customize the setup:

- `HOST_PORT` changes the host HTTP port from `9991`.
- `WEBPACK_PORT` changes the exposed webpack port from `9994`.
- `HELP_CENTER_PORT` changes the exposed help center port from `9995`.
- `UBUNTU_MIRROR` selects an alternative Ubuntu package mirror.

For example:

```console
$ HOST_UID=$(id -u) HOST_PORT=8080 docker compose up --build
```

Run development commands inside the environment with `docker compose exec`:

```console
$ docker compose exec zulip bash -lc './tools/lint path/to/changed/file'
```
