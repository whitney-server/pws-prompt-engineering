# Task: Dockerize an Application

## Objective
Configure this project to build and deploy with Docker, behind a Traefik reverse proxy.

## Deliverables
1. Create a `Dockerfile` at the repository root that builds a production image.
2. Create a `docker-compose.yml` that runs the image and exposes it to Traefik.
3. Clean your work to remove extraneous comments. At most, you may include short, one liner comments for complex concepts.

## Placeholders
Replace these before use; they are not real values.

| Placeholder | Meaning | Example |
|-------------|---------|---------|
| `<DOMAIN>` | Public domain the app is served at | `xxx.example.com` |
| `<SERVICE_NAME>` | Short name for the container/service | `myapp` |
| `<ROUTER_NAME>` | Unique Traefik router label key | `myapp` |
| `<INTERNAL_PORT>` | Port the app listens on inside the container | `80` |

## Dockerfile
- Copy in only what is needed to build and run the app.
- Identify the project's correct build process and include the relevant build steps to produce a production-ready artifact.
- Prefer a multi-stage build: compile/build in a build stage, then copy only the resulting artifact into a minimal runtime image.
- Inject any build-time configuration or secrets as build args, not hardcoded values.
- Expose the port the app serves on (`<INTERNAL_PORT>`).
- Define a container `HEALTHCHECK` that probes the app so the orchestrator can tell when it is live.

## Docker Compose
- Define a single service for the application that builds from the `Dockerfile` (passing any required build args).
- Use a sensible `restart` policy (e.g. `unless-stopped`).
- Set a stable `container_name` so deploys are predictable.

### Traefik integration
- Deployment uses **Traefik** as the reverse proxy; the container is discovered via the shared Docker network, not by publishing ports.
- Attach the service to the external Docker network named `traefik` (declare it as `external: true` — Traefik already owns it).
- Add Traefik labels so the proxy routes `<DOMAIN>` to this container:
  - Enable Traefik for the service.
  - Define a router whose rule matches `Host(\`<DOMAIN>\`)`.
  - Enable TLS on the router (with whatever cert resolver Traefik is configured to use).
  - Point the load balancer at the app's internal port (`<INTERNAL_PORT>`).

### Reference shape
```yaml
networks:
  traefik:
    external: true

services:
  <SERVICE_NAME>:
    container_name: <SERVICE_NAME>
    build:
      context: .
      args:
        # build-time config/secrets, if any
    restart: unless-stopped
    networks:
      - traefik
    labels:
      - traefik.enable=true
      - traefik.http.routers.<ROUTER_NAME>.rule=Host(`<DOMAIN>`)
      - traefik.http.routers.<ROUTER_NAME>.tls=true
      - traefik.http.routers.<ROUTER_NAME>.tls.certresolver=lets-encrypt
      - traefik.http.services.<ROUTER_NAME>.loadbalancer.server.port=<INTERNAL_PORT>
```

## Conventions
- Keep image contents minimal — copy only what the build and runtime require.
- Never hardcode secrets in the image or compose file; pass them as build args or environment values sourced from secrets.
- Keep the steps language- and stack-agnostic; adapt concrete commands to whatever this project uses.
