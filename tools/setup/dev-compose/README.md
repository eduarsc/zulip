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
- `EXTERNAL_HOST` sets the host and optional port used in browser-facing URLs.
  It defaults to `localhost:9991`.
- `BEHIND_HTTPS_PROXY=1` generates HTTPS URLs when a reverse proxy terminates
  TLS in front of the development server.
- `UBUNTU_MIRROR` selects an alternative Ubuntu package mirror.
- `PROXY_URL` changes the HTTP/HTTPS proxy from
  `http://128.0.205.252:3128`; set it to an empty value to disable the proxy.
- `NO_PROXY` changes the hosts that bypass the proxy. Its default includes all
  services used through the container's loopback interface. The development
  image preserves these proxy variables when provisioning commands use `sudo`.
  `NODE_USE_ENV_PROXY` is enabled so Node and Corepack use the same proxy.

For example:

```console
$ EXTERNAL_HOST=testing.example.com:9991 docker compose up --build
```

When using an HTTPS reverse proxy on its standard port, omit the port and run
`BEHIND_HTTPS_PROXY=1 EXTERNAL_HOST=testing.example.com docker compose up`.

Run development commands inside the environment with `docker compose exec`:

```console
$ docker compose exec zulip bash -lc './tools/lint path/to/changed/file'
```
