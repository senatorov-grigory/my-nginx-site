# Nginx Podman Learning Journal

A static learning journal served by Nginx inside a Podman container.

The website documents my daily progress with Linux, Podman, container images, bind mounts, volumes, Git and GitHub.

## Technologies

- Linux
- Podman
- Nginx
- HTML
- CSS
- Git
- GitHub

## Project Structure

```text
my-nginx-site/
├── site/
│   ├── index.html
│   └── style.css
├── Containerfile
├── .containerignore
└── README.md
```

## Website Files

- `site/index.html` contains the structure and learning journal.
- `site/style.css` contains the visual design.
- `Containerfile` describes how the container image is built.
- `.containerignore` excludes unnecessary files from the build context.

## Build the Image

Run the following command inside the project directory:

```bash
podman build --format docker -t my-nginx-site:1.3 .
```

Command explanation:

- `podman build` builds a new container image.
- `--format docker` stores health-check metadata in Docker image format.
- `-t my-nginx-site:1.3` assigns a name and version tag.
- `.` uses the current directory as the build context.

## Run the Container

```bash
podman run -d \
  --name my-site \
  -p 8080:80 \
  my-nginx-site:1.3
```

The website is available at:

```text
http://SERVER_IP:8080
```

Port `8080` belongs to the server, while port `80` belongs to Nginx inside the container.

## Development with a Bind Mount

A bind mount makes it possible to edit the website without rebuilding the image:

```bash
podman run -d \
  --name my-site-bind \
  -p 8081:80 \
  -v "$PWD/site:/usr/share/nginx/html:ro" \
  docker.io/library/nginx:alpine
```

The development version is available at:

```text
http://SERVER_IP:8081
```

Changes made to `site/index.html` or `site/style.css` become visible after refreshing the browser.

The `:ro` option gives the container read-only access to the website files.

## Persistent Storage with a Named Volume

Create a Podman-managed volume:

```bash
podman volume create nginx-site-data
```

Run a container with the volume:

```bash
podman run -d \
  --name my-site-volume \
  -p 8082:80 \
  -v nginx-site-data:/usr/share/nginx/html \
  my-nginx-site:1.3
```

A named volume stores data separately from the container. Its data can remain available after the container is removed.

## Useful Commands

Show running containers:

```bash
podman ps
```

Show all containers:

```bash
podman ps -a
```

Show local images:

```bash
podman images
```

Show container logs:

```bash
podman logs my-site
```

Inspect container health:

```bash
podman inspect my-site
```

Stop and start the container:

```bash
podman stop my-site
podman start my-site
```

Remove the container:

```bash
podman rm -f my-site
```

## Health Check

The image includes a health check that periodically verifies whether Nginx responds inside the container.

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --quiet --spider http://127.0.0.1/ || exit 1
```

The container status can be checked with:

```bash
podman ps
```

A working container should display:

```text
Up (healthy)
```

## Learning Goals

This project demonstrates:

- Creating and managing containers
- Building custom container images
- Using version tags
- Publishing container ports
- Reading container logs
- Configuring health checks
- Using bind mounts for development
- Using named volumes for persistent data
- Tracking changes with Git
- Publishing a project on GitHub

## Author

Grigory Senatorov
