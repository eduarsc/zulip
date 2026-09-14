# Docker Compose development environment

This Compose configuration runs the Zulip development server from the current
checkout. It uses the same image and provisioning flow as Zulip's supported
Vagrant-with-Docker environment.

Build and start the environment:

```console
$ docker compose up --build
```

The entrypoint matches the container's `vagrant` user to the owner of the
checkout, so the repository must be owned by the non-root user that runs Docker
Compose.

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
- `PROXY_URL` changes the HTTP/HTTPS proxy from
  `http://128.0.205.252:3128`; set it to an empty value to disable the proxy.
- `NO_PROXY` changes the hosts that bypass the proxy. Its default includes all
  services used through the container's loopback interface. The development
  image preserves these proxy variables when provisioning commands use `sudo`.

For example:

```console
$ HOST_PORT=8080 docker compose up --build
```

Run development commands inside the environment with `docker compose exec`:

```console
$ docker compose exec zulip bash -lc './tools/lint path/to/changed/file'
```
