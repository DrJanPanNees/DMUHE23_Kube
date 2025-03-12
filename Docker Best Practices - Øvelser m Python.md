# Docker Best Practices - Øvelser (Python-version)

## Metadata

- **Emne:** Docker Best Practices
- **Niveau:** Mellem til avanceret
- **Forudsætninger:** Grundlæggende kendskab til Docker, herunder Dockerfiles og containerhåndtering
- **Varighed:** 1-2 timer afhængigt af dybden
- **Læringsmål:**
  - Docker best practices
  - Optimér Docker-images
  - Anvend caching og multi-stage builds
  - Sikre Docker-miljøer

---

## Øvelse 1: Brug officielle og versionerede billeder

**Opgaver:**

- Opret mappe kaldet `myapp` og naviger ind i den:
  ```bash
  mkdir myapp && cd myapp
  ```
- Opret en fil kaldet `requirements.txt` med følgende indhold:
  ```
  flask==2.0.2
  ```
- Opret en fil kaldet `app.py` med følgende indhold:
  ```python
  from flask import Flask
  app = Flask(__name__)

  @app.route("/")
  def hello():
      return "Hello, World!"

  if __name__ == "__main__":
      app.run(debug=True, host="0.0.0.0")
  ```
- Skriv Dockerfile:
  ```dockerfile
  FROM python:3.9-alpine
  WORKDIR /app
  COPY requirements.txt .
  RUN pip install --no-cache-dir -r requirements.txt
  COPY . .
  CMD ["python", "app.py"]
  ```
- Byg og kør:
  ```bash
  docker build -t myapp .
  docker run -p 5000:5000 myapp
  ```

---

## Øvelse 2: Optimering af caching i Dockerfile

- Samme filer som Øvelse 1.
- Strukturér Dockerfile for at installere afhængigheder før kildekode for caching.
- Observer caching-effekten.

---

## Øvelse 3: Brug `.dockerignore`

**Opgaver:**

- Navigér til projektmappen:
  ```bash
  cd myapp
  ```
- Opret en fil kaldet `.dockerignore` i samme mappe som din Dockerfile med følgende indhold:
  ```
  __pycache__/
  logs/
  .git/
  .DS_Store
  .env
  ```
- Kontrollér, at filen er oprettet korrekt:
  ```bash
  ls -a
  ```
- Byg dit Docker-image:
  ```bash
  docker build -t myapp .
  ```
- Slet eller omdøb midlertidigt `.dockerignore` filen og byg imaget igen:
  ```bash
  mv .dockerignore dockerignore_old
  docker build -t myapp_nodockerignore .
  ```
- Sammenlign størrelserne på de to images:
  ```bash
  docker images | grep myapp
  ```

---

## Øvelse 4: Multi-stage build for at optimere image-størrelse

**Beskrivelse:**

Multi-stage builds reducerer størrelsen på Docker-images ved at adskille byggefasen fra det endelige runtime-miljø. Dette eksempel tydeliggør effekten af multi-stage builds.

**Opgaver:**

1. **Simuler byggeproces:**

- Opret `build.py` med:
```python
with open("largefile.tmp", "wb") as f:
    f.seek(1024 * 1024 * 50 - 1)  # 50 MB
    f.write(b"\0")
print("Build complete")
```

- Tilføj til `requirements.txt`:
```
flask==2.0.2
requests==2.27.1
```

- Dockerfile (uden multi-stage):
```dockerfile
FROM python:3.9-alpine
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
RUN python build.py
CMD ["python", "app.py"]
```

- Byg og noter størrelse:
```bash
docker build -t myapp_standard .
docker images | grep myapp_standard
```

2. **Multi-stage Dockerfile:**
```dockerfile
FROM python:3.9-alpine AS builder
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
RUN python build.py

FROM python:3.9-alpine
WORKDIR /app
COPY --from=builder /app /app
RUN rm largefile.tmp
CMD ["python", "app.py"]
```

- Byg multi-stage image:
```bash
docker build -t myapp_multistage .
docker images | grep myapp_multistage
```
- Sammenlign størrelsen med det første image.

**Diskussion:**
- Hvorfor er multi-stage en fordel?
- Hvordan kan du anvende multi-stage builds i praksis?

---

## Øvelse 5: Kør ikke containere som root

**Opgaver:**

- Dockerfile med ikke-root bruger:
```dockerfile
FROM python:3.9-alpine
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
COPY . .
CMD ["python", "app.py"]
```

---

## Øvelse 6: Docker Scout

**Opgaver:**

- Scan image med Docker Scout:
```bash
docker scout report myapp
```

---

## Afsluttende refleksion

Diskuter:

- Effekten på byggetid og image-størrelse
- Anvendelse i DevOps
- Automatisering af sikkerhed og optimering

---

**Ekstra Ressourcer:**

- [Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker Security Guide](https://docs.docker.com/engine/security/)

