# Hello World App — Containerized Setup

This project provides a containerized version of a Python (Flask) application using Docker.

---

## 📦 Prerequisites

Make sure you have the following installed:

* Docker Engine
* Docker Compose (v2)

Verify installation:

```bash
docker --version
docker compose version
```

---

## 🏗️ Pull the Docker Image

The application image is hosted on Docker Hub:

```bash
docker pull itaygeron2/hello-world-app:v1.0
```

---

## ▶️ Run the Container (Standalone)

```bash
docker run -d \
  --name hello-world-app \
  -p 5000:5000 \
  itaygeron2/hello-world-app:v1.0
```

Access the application at:

```text
http://localhost:5000
```

---

## 🐳 Run with Docker Compose

Example `docker-compose.yml`:

```yaml
services:
  app:
    image: itaygeron2/hello-world-app:v1.0
    container_name: hello-world-app
    ports:
      - "5000:5000"
    volumes:
      - app_data:/app/data

volumes:
  app_data:
```

Run:

```bash
docker compose up -d
```

---

## 📜 View Logs

Standalone container:

```bash
docker logs -f hello-world-app
```

Docker Compose:

```bash
docker compose logs -f
```

---

## ⏹️ Stop and Clean Up

Standalone:

```bash
docker stop hello-world-app
docker rm hello-world-app
```

Docker Compose:

```bash
docker compose down
```

---

## ⚠️ Notes

* Ensure the application inside the container listens on all interfaces:

```python
app.run(host="0.0.0.0", port=5000)
```

---

## 🚀 Summary

* Pull image → `docker pull itaygeron2/hello-world-app:v1.0`
* Run container → `docker run` or `docker compose up`
* Access app → `http://localhost:5000`
