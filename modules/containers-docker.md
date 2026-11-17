# Module: containers with Docker

Load when they work on Dockerfiles, images, Docker Compose, or running the project locally in containers. Teaching rules from `SKILL.md` apply: explain concepts and give exact tool commands directly, but never write their Dockerfile or compose file. Log in `log/containers.md`.

Their project already has a `Dockerfile` per service and a `docker-compose.yml` under `infra/docker/`, so they will usually be changing and extending files that exist rather than writing from scratch.

**On their machine:** Fedora, where Podman is installed and Docker is not (checked 2026-11-17; check again before relying on it). Podman reads Dockerfiles and compose files, so the files are the same; the engine differs (Docker runs a root daemon, Podman is daemonless and rootless by default). Tell them this once, say which tool a command is for, and let them choose whether to install Docker.

## What to teach, in order

1. Image vs container vs volume vs network, and why a container is a process, not a VM.
2. A Dockerfile for a Rust binary: layers, build cache, multi-stage builds, why the final image doesn't carry the compiler, and the dependency-caching problem with `cargo`.
3. Docker Compose (`docker compose`, the v2 plugin): services, networks, `depends_on` and what it does not guarantee (a started container is not a ready database, so use health checks), environment, `.env` files.
4. The difference between a development compose file (bind mounts, hot reload, dev servers) and a production image (built artefacts, no source mounted, no dev server).
5. Persistent data: named volumes, where Postgres data lives, backups.
6. Security basics: the Docker daemon and its socket are root-equivalent, rootless mode, non-root users inside images, capabilities.

## Best practices to hold their work to

- Run the process inside the container as an unprivileged user, not root (`USER` in the Dockerfile).
- Pin image tags (no `latest`) and say why: builds that silently change.
- Small base images, multi-stage builds, one process per container, no build tools in the final image. Use a `.dockerignore` so `target/`, `node_modules/` and `.git/` don't enter the build context.
- No secrets in images, in the Dockerfile, or in committed compose files. Use environment injection or secrets mechanisms, and keep real values out of git. Default passwords in a committed compose file are acceptable only for a throwaway local database, and the file should say so.
- Don't publish the database port to the network unless a local tool needs it, and then bind it to `127.0.0.1`. Only the ports that need to be reachable are published.
- Add health checks, and resource limits where it matters.
- Never mount the Docker socket into a container, and don't add them to the `docker` group without saying it is root-equivalent.

## Safety protocols

- Before any command that removes containers, volumes or images, say what it will delete. `docker compose down -v` and `docker system prune --volumes` delete data volumes, including a local Postgres.
- The Postgres data volume holds work that took time to build. Tell them to dump (`pg_dump`) before recreating it.
- Don't bind-mount their home directory into a container.
- A dev compose file with bind mounts gives the container write access to the source on the host; say so when it comes up.

## Sources (general practice, not searched; verify against the Docker docs, `docs.docker.com`)

- The points above are standard Docker guidance written from general knowledge. Check them against the Docker documentation (build best practices, rootless mode, compose) when they matter, and say so when you can't back a claim.
