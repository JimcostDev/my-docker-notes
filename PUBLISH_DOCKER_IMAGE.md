# 🐳 Publicar una imagen en Docker Hub

## 🔹 1. Iniciar sesión en Docker Hub

```bash
docker login
```

> Te pedirá tu **usuario** (`jimcostdev`) y tu **contraseña o token de acceso** (recomendado generar uno desde tu cuenta de Docker Hub).

---

## 🔹 2. Construir la imagen con tu nombre de usuario y repositorio

```bash
docker build -t jimcostdev/go-api:latest .
```

**Explicación:**
- `-t` → asigna una etiqueta a la imagen (`tag`).
- `jimcostdev/go-api` → nombre del repositorio que aparecerá en tu cuenta.
- `:latest` → etiqueta de versión (puedes usar `:v1`, `:prod`, etc.).
- `.` → indica el directorio donde está tu `Dockerfile`.

---

## 🔹 3. Verificar la imagen creada

```bash
docker images
```

**Ejemplo de salida:**
```
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
jimcostdev/go-api       latest    4a1b2c3d4e5f   1 minute ago     120MB
```

---

## 🔹 4. Subir la imagen a Docker Hub

```bash
docker push jimcostdev/go-api:latest
```

✅ Esto subirá la imagen al repositorio:  
👉 [https://hub.docker.com/r/jimcostdev/go-api](https://hub.docker.com/r/jimcostdev/go-api)

---

## 🔹 5. Descargar la imagen en otra máquina

```bash
docker pull jimcostdev/go-api:latest
```

Y luego puedes ejecutarla con:

```bash
docker run -d -p 8080:8080 jimcostdev/go-api:latest
```

---

## 🧩 Tips adicionales

- Usa **tags** claros para tus versiones:  
  ```bash
  docker build -t jimcostdev/go-api:v1.0.0 .
  docker push jimcostdev/go-api:v1.0.0
  ```

- Si haces cambios, **reconstruye** y **vuelve a hacer push**:
  ```bash
  docker build -t jimcostdev/go-api:latest .
  docker push jimcostdev/go-api:latest
  ```

---

📘 **Referencia:** [https://docs.docker.com/docker-hub/](https://docs.docker.com/docker-hub/)
