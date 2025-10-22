# 🐳 Mi Guía Completa de Docker


## 📋 Tabla de Contenidos
- [Conceptos Básicos](#-conceptos-básicos)
- [Imágenes](#️-gestión-de-imágenes)
- [Contenedores](#-gestión-de-contenedores)
- [Redes](#-redes)
- [Volúmenes](#-volúmenes)
- [Docker Compose](#-docker-compose)
- [Dockerfile](#-dockerfile)
- [Comandos de Limpieza](#️-comandos-de-limpieza)
- [Tips y Mejores Prácticas](#-tips-y-mejores-prácticas)

---

## 🎯 Conceptos Básicos

### ¿Qué es Docker?
Docker es una plataforma que permite crear, desplegar y ejecutar aplicaciones en contenedores. Los contenedores son entornos aislados que incluyen todo lo necesario para que una aplicación funcione.

**Elementos principales:**
- **Imagen**: Plantilla inmutable para crear contenedores
- **Contenedor**: Instancia en ejecución de una imagen
- **Dockerfile**: Archivo de instrucciones para construir imágenes
- **Registry**: Repositorio de imágenes (ej: Docker Hub)

---

## 🖼️ Gestión de Imágenes

### Construir una imagen
```bash
# Construir desde un Dockerfile en el directorio actual
docker build -t nombre-imagen:etiqueta .

# Ejemplo práctico
docker build -t miapp:local .
docker build -t miapp:v1.0.0 .
```

### Listar imágenes
```bash
# Ver todas las imágenes
docker images

# Ver solo IDs de imágenes
docker images -q

# Filtrar imágenes
docker images miapp
```

### Descargar imágenes
```bash
# Descargar desde Docker Hub
docker pull nginx
docker pull node:18-alpine
docker pull postgres:15
```

### Etiquetar imágenes
```bash
# Crear una nueva etiqueta para una imagen existente
docker tag imagen-origen:etiqueta imagen-destino:nueva-etiqueta
docker tag miapp:local miapp:production
```

### Eliminar imágenes
```bash
# Por nombre:etiqueta
docker rmi miapp:local

# Por ID
docker rmi abc12345defg

# Eliminar todas las imágenes no utilizadas
docker image prune

# Eliminar todas las imágenes (¡CUIDADO!)
docker rmi $(docker images -q)
```

---

## 📦 Gestión de Contenedores

### Ejecutar contenedores
```bash
# Básico
docker run nombre-imagen

# Con opciones comunes
docker run --name mi-contenedor --rm -d -p 8000:8000 miapp:local

# Interactivo (para debugging)
docker run -it --rm ubuntu /bin/bash
```

**Opciones importantes:**
- `--name nombre`: Asigna un nombre específico
- `--rm`: Elimina el contenedor al terminar
- `-d`: Ejecuta en segundo plano (detached)
- `-p host:contenedor`: Mapea puertos
- `-v host:contenedor`: Monta volúmenes
- `-e VARIABLE=valor`: Define variables de entorno
- `-it`: Modo interactivo con terminal

### Gestión de contenedores en ejecución
```bash
# Listar contenedores activos
docker ps

# Listar todos los contenedores (activos e inactivos)
docker ps -a

# Ver logs
docker logs nombre-contenedor
docker logs -f nombre-contenedor  # Seguir logs en tiempo real

# Ejecutar comandos en contenedor activo
docker exec -it nombre-contenedor /bin/bash
docker exec nombre-contenedor ls -la

# Detener contenedor
docker stop nombre-contenedor

# Reiniciar contenedor
docker restart nombre-contenedor

# Pausar/reanudar contenedor
docker pause nombre-contenedor
docker unpause nombre-contenedor
```

### Eliminar contenedores
```bash
# Eliminar un contenedor específico
docker rm nombre-contenedor

# Forzar eliminación de contenedor en ejecución
docker rm -f nombre-contenedor

# Eliminar todos los contenedores parados
docker container prune

# Eliminar todos los contenedores (¡CUIDADO!)
docker rm $(docker ps -aq)
```

---

## 🌐 Redes

### Gestión de redes
```bash
# Listar redes
docker network ls

# Crear red personalizada
docker network create mi-red
docker network create --driver bridge mi-red-bridge

# Inspeccionar red
docker network inspect bridge

# Conectar contenedor a red
docker network connect mi-red nombre-contenedor

# Desconectar contenedor de red
docker network disconnect mi-red nombre-contenedor

# Eliminar red
docker network rm mi-red
```

### Ejecutar contenedor en red específica
```bash
docker run --network mi-red --name app miapp:local
```

---

## 💾 Volúmenes

### Tipos de almacenamiento
```bash
# Volumen nombrado (recomendado para datos persistentes)
docker volume create mi-volumen
docker run -v mi-volumen:/data miapp:local

# Bind mount (carpeta del host)
docker run -v /ruta/host:/ruta/contenedor miapp:local
docker run -v $(pwd):/app node:18 npm start

# Volumen temporal (tmpfs)
docker run --tmpfs /tmp miapp:local
```

### Gestión de volúmenes
```bash
# Listar volúmenes
docker volume ls

# Inspeccionar volumen
docker volume inspect mi-volumen

# Eliminar volumen
docker volume rm mi-volumen

# Eliminar volúmenes no utilizados
docker volume prune
```

---

## 🐙 Docker Compose

### Archivo docker-compose.yml básico
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - NODE_ENV=production
    depends_on:
      - db
    volumes:
      - ./src:/app/src

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: miapp
      POSTGRES_USER: usuario
      POSTGRES_PASSWORD: contraseña
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Comandos de Docker Compose
```bash
# Levantar servicios
docker-compose up
docker-compose up -d  # En segundo plano

# Construir y levantar
docker-compose up --build

# Ver logs
docker-compose logs
docker-compose logs nombre-servicio

# Detener servicios
docker-compose down

# Detener y eliminar volúmenes
docker-compose down -v

# Escalar servicios
docker-compose up --scale web=3

# Ejecutar comandos en servicios
docker-compose exec web /bin/bash
```

---

## 📄 Dockerfile

### Estructura básica
```dockerfile
# Imagen base
FROM node:18-alpine

# Directorio de trabajo
WORKDIR /app

# Copiar archivos de dependencias
COPY package*.json ./

# Instalar dependencias
RUN npm ci --only=production

# Copiar código fuente
COPY . .

# Exponer puerto
EXPOSE 8000

# Usuario no root (seguridad)
USER node

# Comando por defecto
CMD ["npm", "start"]
```

### Comandos Dockerfile más usados
- `FROM`: Imagen base
- `WORKDIR`: Directorio de trabajo
- `COPY`: Copiar archivos del host al contenedor
- `ADD`: Similar a COPY pero con más funciones
- `RUN`: Ejecutar comandos durante la construcción
- `CMD`: Comando por defecto al ejecutar contenedor
- `ENTRYPOINT`: Punto de entrada fijo
- `EXPOSE`: Documentar puertos expuestos
- `ENV`: Variables de entorno
- `USER`: Usuario para ejecutar procesos
- `VOLUME`: Puntos de montaje

### Multi-stage builds
```dockerfile
# Etapa de construcción
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Etapa de producción
FROM nginx:alpine AS production
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 🗑️ Comandos de Limpieza

### Limpieza general
```bash
# Eliminar todo lo no utilizado (contenedores, redes, imágenes, cache)
docker system prune

# Limpieza agresiva (incluyendo volúmenes)
docker system prune -a --volumes

# Ver uso de espacio
docker system df
```

### Limpieza específica
```bash
# Contenedores parados
docker container prune

# Imágenes sin usar
docker image prune

# Redes sin usar
docker network prune

# Volúmenes sin usar
docker volume prune

# Cache de build
docker builder prune
```

---

## 💡 Tips y Mejores Prácticas

### Seguridad
```bash
# Usar usuario no root
USER node

# Escanear vulnerabilidades
docker scout quickview imagen:tag

# Variables de entorno seguras
docker run --env-file .env miapp:local
```

### Performance
```bash
# Usar .dockerignore
echo "node_modules" >> .dockerignore
echo ".git" >> .dockerignore
echo "*.log" >> .dockerignore

# Multi-stage builds para imágenes más pequeñas
# Usar imágenes Alpine cuando sea posible
# Ordenar comandos RUN del menos al más cambiante
```

### Debugging
```bash
# Ver procesos dentro del contenedor
docker top nombre-contenedor

# Estadísticas de recursos
docker stats nombre-contenedor

# Inspeccionar contenedor
docker inspect nombre-contenedor

# Historial de imagen
docker history imagen:tag
```

### Aliases útiles (agregar a ~/.bashrc o ~/.zshrc)
```bash
# Shortcuts para comandos frecuentes
alias dps='docker ps'
alias dpa='docker ps -a'
alias di='docker images'
alias dexec='docker exec -it'
alias dlogs='docker logs -f'
alias dclean='docker system prune -f'

# Docker Compose shortcuts
alias dc='docker-compose'
alias dcu='docker-compose up'
alias dcd='docker-compose down'
alias dcb='docker-compose build'
```

---

## 🚀 Comandos de Uso Diario

### Flujo de desarrollo típico
```bash
# 1. Construir imagen
docker build -t miapp:dev .

# 2. Ejecutar con desarrollo
docker run --name miapp-dev --rm -p 8000:8000 -v $(pwd):/app miapp:dev

# 3. Ver logs
docker logs -f miapp-dev

# 4. Ejecutar comandos dentro del contenedor
docker exec -it miapp-dev /bin/bash

# 5. Limpiar al final del día
docker system prune -f
```

### Troubleshooting común
```bash
# Contenedor no inicia
docker logs nombre-contenedor

# Puerto ocupado
lsof -i :8000  # Ver qué usa el puerto
docker run -p 8001:8000 miapp:local  # Cambiar puerto

# Imagen muy grande
docker history imagen:tag  # Ver capas
# Usar multi-stage builds y .dockerignore

# Contenedor lento
docker stats  # Ver recursos
# Ajustar límites de memoria/CPU
```

---

## 📚 Recursos Adicionales

- [Documentación oficial de Docker](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- [Docker Compose reference](https://docs.docker.com/compose/compose-file/)




