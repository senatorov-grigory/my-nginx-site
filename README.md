# Nginx Podman Website

A simple static website served by Nginx inside a Podman container.

This project was created to practice containerization, image building, port mapping, and basic Git workflows.

## Technologies

- Linux
- Podman
- Nginx
- HTML
- Git
- GitHub

## Project Structure

```text
.
├── Containerfile
├── index.html
├── README.md
└── .containerignore
```

## Build the Image

Run the following command inside the project directory:

```bash
podman build -t my-nginx-site:1.0 .
```

## Run the Container

```bash
podman run -d --name my-site -p 8080:80 my-nginx-site:1.0
```

The website will be available at:

```text
http://localhost:8080
```

When running the container on a remote virtual machine, use the VM IP address:

```text
http://VM_IP_ADDRESS:8080
```

## Check the Running Container

```bash
podman ps
```

## View Container Logs

```bash
podman logs my-site
```

## Stop the Container

```bash
podman stop my-site
```

## Start the Existing Container

```bash
podman start my-site
```

## Remove the Container

```bash
podman rm -f my-site
```

## Learning Goals

This project demonstrates:

- Building a container image from a Containerfile
- Running Nginx inside a container
- Mapping a host port to a container port
- Managing container lifecycle
- Tracking project changes with Git
- Publishing source code on GitHub
