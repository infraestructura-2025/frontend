# frontend
Aplicación frontend para registro de usuarios

README para infraestructura-2025/frontend

Aplicación web frontend para el sistema de registro de usuarios de infraestructura-2025. Esta es una Single Page Application (SPA) vanilla JavaScript servida mediante Nginx en un contenedor Docker.

CARACTERÍSTICAS
Registro de usuarios: Formulario para crear nuevos usuarios con nombre, email y teléfono
Lista de usuarios: Visualización en tiempo real de todos los usuarios registrados
Notificaciones: Mensajes de éxito/error con auto-dismiss
Integración con backend: Comunicación con API REST mediante proxy inverso

STACK TECNOLÓGICO
Frontend: HTML5, CSS3, JavaScript vanilla (ES2017+)
Servidor Web: Nginx (Alpine Linux)
Containerización: Docker
CI/CD: GitHub Actions + SonarCloud

DEPENDENCIAS Y REQUISITOS PREVIOS

Software Requerido:
Docker Engine (versión 20.x o superior)
Instalación en Ubuntu/Debian:
sudo apt-get update
sudo apt-get install docker.io
sudo systemctl start docker
sudo systemctl enable docker

Instalación en macOS: Docker Desktop
Instalación en Windows: Docker Desktop

Git (para clonar el repositorio)
sudo apt-get install git

Dependencias del Sistema
El proyecto utiliza las siguientes dependencias que se instalan automáticamente dentro del contenedor Docker:
Imagen base: nginx:alpine
Nginx: Servidor web (incluido en la imagen base)


Dependencias de Desarrollo (Opcional)
Para desarrollo y análisis de código:
Node.js 18.x 
SonarScanner (se instala automáticamente en GitHub Actions)
Dependencias Externas
Backend API: Debe estar corriendo en backend:8000
Endpoints requeridos:
POST /api/users/ - Crear usuario
GET /api/users/list/ - Listar usuarios

INICIO RÁPIDO

Clonar el repositorio
git clone https://github.com/infraestructura-2025/frontend.git
cd frontend

Construcción de la imagen Docker
docker build -t frontend .

Ejecución del contenedor
Opción A:
Con red Docker personalizada (recomendado)
 Crear red si no existe: docker network create app-network
Ejecutar frontend (asumiendo que backend ya está en la misma red):          docker run -d -p 80:80 --name frontend --network app-network frontend

Opción B: Con host network
docker run -d -p 80:80 --name frontend --network=host frontend
Acceder a la aplicación
Abre tu navegador en: http://localhost

ARQUITECTURA

La aplicación utiliza Nginx para dos propósitos:
Servir archivos estáticos: El archivo index.html se sirve directamente
Proxy inverso: Las peticiones a /api/* se redirigen al backend en backend:8000

Flujo de Peticiones:
Usuario → Navegador → http://localhost:80 → Nginx Container → index.html y /api/* → backend:8000

Endpoints del Backend:
POST /api/users/ - Crear nuevo usuario
GET /api/users/list/ - Obtener lista de usuarios

ESTRUCTURA DEL PROYECTO
frontend/
index.html: Aplicación SPA completa (141 líneas)
nginx.conf: Configuración de Nginx (35 líneas)
Dockerfile: Definición del contenedor (4 líneas)
.github/workflows/sonarcloud.yml: Pipeline de calidad de código
LICENSE: Licencia MIT
README.md: Este archivo

CONFIGURACION
Nginx
La configuración de Nginx incluye:
Servidor en puerto 80
Root directory: /usr/share/nginx/html
Proxy pass a backend:8000 para rutas /api/*
Headers CORS configurados para permitir peticiones cross-origin
Manejo de preflight requests (OPTIONS)

Variables de Entorno
No se requieren variables de entorno. La configuración del backend se maneja mediante la red Docker.

CALIDAD DE CODIGO
El proyecto utiliza SonarCloud para análisis estático de código. Los escaneos se ejecutan automáticamente en:
Push a ramas main o master
Pull requests a ramas main o master
Configuración de SonarCloud:
Organización: infraestructura-2025
Proyecto: infraestructura-2025_frontend

DESARROLLO
Modificar la aplicación
Edita index.html para cambiar la interfaz o lógica de la aplicación. No se requiere proceso de build.
Probar localmente
Construir imagen: docker build -t frontend .
Ejecutar (asegúrate de que el backend esté corriendo): docker run -p 80:80 --network=host frontend
Ver logs: docker logs -f frontend
Detener contenedor: docker stop frontend && docker rm frontend

Comandos Utiles
Ver contenedores en ejecución: docker ps
Inspeccionar red Docker: docker network inspect app-network
Reconstruir sin caché: docker build --no-cache -t frontend .
Ejecutar en modo interactivo: docker run -it -p 80:80 frontend /bin/sh



