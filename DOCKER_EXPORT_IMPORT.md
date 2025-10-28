# 📦 Exportar e Importar Imágenes y Contenedores en Docker

## 🔹 1. Guardar una imagen en un archivo `.tar`

```bash
docker save -o mi_imagen.tar nombre_imagen:tag
```

**Ejemplo:**
```bash
docker save -o go-api.tar jimcostdev/go-api:latest
```

> 📁 Esto crea un archivo `go-api.tar` que contiene la imagen completa, lista para transferir o respaldar.

---

## 🔹 2. Cargar una imagen desde un archivo `.tar`

```bash
docker load -i mi_imagen.tar
```

**Ejemplo:**
```bash
docker load -i go-api.tar
```

> ✅ Esto restaurará la imagen en tu sistema con el mismo nombre y etiqueta que tenía originalmente.

---

## 🔹 3. Exportar un contenedor en ejecución o detenido

```bash
docker export -o mi_contenedor.tar nombre_contenedor
```

**Ejemplo:**
```bash
docker export -o contenedor_api.tar go-api-container
```

> ⚠️ `docker export` guarda **solo el sistema de archivos del contenedor**, no los metadatos de la imagen (volúmenes, variables de entorno, etc.).

---

## 🔹 4. Importar un contenedor exportado

```bash
docker import mi_contenedor.tar nombre_imagen:tag
```

**Ejemplo:**
```bash
docker import contenedor_api.tar jimcostdev/go-api:imported
```

> 🧠 Esto crea una **nueva imagen** a partir del sistema de archivos exportado del contenedor.

---

## 🔹 5. Diferencias entre `save/load` y `export/import`

| Comando | Qué guarda | Qué restaura | Ideal para |
|----------|-------------|---------------|-------------|
| `save` / `load` | Imagen Docker (con metadatos y capas) | Imagen original | Respaldar o transferir imágenes completas |
| `export` / `import` | Contenedor (solo sistema de archivos) | Nueva imagen simplificada | Migrar contenedores ligeros o limpiar capas |

---

## 🔹 6. Verificar imágenes disponibles

```bash
docker images
```

Y para verificar contenedores existentes:

```bash
docker ps -a
```

---

📘 **Referencia oficial:**  
[https://docs.docker.com/engine/reference/commandline/save/](https://docs.docker.com/engine/reference/commandline/save/)  
[https://docs.docker.com/engine/reference/commandline/export/](https://docs.docker.com/engine/reference/commandline/export/)
