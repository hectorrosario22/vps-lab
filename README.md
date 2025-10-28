# 📝 Task Manager

Una aplicación fullstack simple de gestión de tareas construida con React, Node.js/Express y PostgreSQL, completamente dockerizada.

## 🚀 Stack Tecnológico

- **Frontend**: React 18 + Vite
- **Backend**: Node.js + Express
- **Base de Datos**: PostgreSQL 15
- **Containerización**: Docker + Docker Compose

## 📋 Características

- ✅ Crear nuevas tareas
- ✅ Listar todas las tareas
- ✅ Marcar tareas como completadas
- ✅ Eliminar tareas
- ✅ Interfaz moderna y responsive
- ✅ API RESTful
- ✅ Completamente dockerizado

## 🛠️ Requisitos Previos

- Docker
- Docker Compose

## 🚀 Inicio Rápido

### 1. Clonar el repositorio

```bash
git clone <repository-url>
cd vps-lab
```

### 2. Levantar los servicios con Docker Compose

```bash
docker-compose up -d
```

Esto iniciará tres servicios:
- **Frontend**: http://localhost:3000 (puerto 3000)
- **Backend API**: http://localhost:5000
- **PostgreSQL**: puerto 5432

### 3. Acceder a la aplicación

Abre tu navegador y ve a: http://localhost:3000

## 📁 Estructura del Proyecto

```
vps-lab/
├── backend/                 # API Express
│   ├── server.js           # Punto de entrada del servidor
│   ├── db.js               # Configuración de PostgreSQL
│   ├── package.json        # Dependencias del backend
│   ├── Dockerfile          # Imagen Docker del backend
│   └── .env.example        # Variables de entorno de ejemplo
├── frontend/               # Aplicación React
│   ├── src/
│   │   ├── App.jsx         # Componente principal
│   │   ├── App.css         # Estilos
│   │   ├── main.jsx        # Punto de entrada
│   │   └── index.css       # Estilos globales
│   ├── package.json        # Dependencias del frontend
│   ├── Dockerfile          # Imagen Docker del frontend (multi-stage)
│   ├── nginx.conf          # Configuración de Nginx
│   └── vite.config.js      # Configuración de Vite
└── docker-compose.yml      # Orquestación de servicios
```

## 🔧 Comandos Útiles

### Ver logs de los servicios

```bash
# Todos los servicios
docker-compose logs -f

# Solo el backend
docker-compose logs -f backend

# Solo el frontend
docker-compose logs -f frontend
```

### Detener los servicios

```bash
docker-compose down
```

### Detener y eliminar volúmenes (reinicio completo)

```bash
docker-compose down -v
```

### Reconstruir las imágenes

```bash
docker-compose up -d --build
```

## ⚙️ Configuración con Dokploy / Variables

Este `docker-compose.yml` está parametrizado para que puedas configurar dominios, puertos y credenciales desde Dokploy o con un archivo `.env` en el raíz.

Variables soportadas (con defaults):

### Base de Datos
- `DB_HOST` (default `db`)
- `DB_PORT` (default `5432`)
- `DB_USER` (default `postgres`)
- `DB_PASSWORD` (default `postgres`)
- `DB_NAME` (default `taskmanager`)

### CORS y API URL
- `CORS_ORIGINS` (default `https://labs-taskmanager-web.hrosario.dev,http://localhost:5173,http://localhost:3000`)
- `VITE_API_URL` (default `https://labs-taskmanager-api.hrosario.dev`)

Copia `.env.example` a `.env` o define estas variables en la UI de Dokploy para el deployment.

### Arquitectura de Dominios

Este proyecto usa **dominios separados** para frontend y backend:

- **Frontend**: `labs-taskmanager-web.hrosario.dev`
- **Backend API**: `labs-taskmanager-api.hrosario.dev`

**Configuración DNS requerida en Cloudflare:**
```
Tipo: A | Nombre: labs-taskmanager-web  | IP: [TU_SERVIDOR] | Proxy: Activado
Tipo: A | Nombre: labs-taskmanager-api  | IP: [TU_SERVIDOR] | Proxy: Activado
```

**SSL/TLS Settings:**
- Encryption mode: "Full (strict)"
- Always Use HTTPS: Activado

## 🌐 API Endpoints

### Health Check
- `GET /health` - Verificar estado del servidor

### Tasks
- `GET /api/tasks` - Obtener todas las tareas
- `POST /api/tasks` - Crear una nueva tarea
  ```json
  {
    "title": "Mi nueva tarea"
  }
  ```
- `PUT /api/tasks/:id` - Actualizar tarea (completar/descompletar)
  ```json
  {
    "completed": true
  }
  ```
- `DELETE /api/tasks/:id` - Eliminar tarea

## 🔐 Variables de Entorno

### Backend (.env)
```env
PORT=5000
DB_HOST=db
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=taskmanager
```

### Frontend (.env)
```env
VITE_API_URL=https://labs-taskmanager-api.hrosario.dev
```

**Nota**: La variable `VITE_API_URL` debe configurarse como **build argument** en Docker. El frontend en producción llama directamente al dominio API separado.

### Backend (.env adicional)
```env
CORS_ORIGINS=https://labs-taskmanager-web.hrosario.dev,http://localhost:5173
```

## 🐳 Deployment en Dokploy

Este proyecto está optimizado para deployment en Dokploy:

### Paso 1: Configurar DNS en Cloudflare

Crea dos registros DNS tipo A:
- `labs-taskmanager-web` → IP de tu servidor Dokploy (Proxy: Activado ☁️)
- `labs-taskmanager-api` → IP de tu servidor Dokploy (Proxy: Activado ☁️)

### Paso 2: Configurar SSL/TLS en Cloudflare

En la sección SSL/TLS de Cloudflare:
- **Encryption mode**: "Full (strict)"
- **Always Use HTTPS**: Activado
- **Minimum TLS Version**: 1.2

### Paso 3: Deployment en Dokploy

1. Sube el proyecto a un repositorio Git
2. En Dokploy, crea una nueva aplicación tipo "Docker Compose"
3. Conecta tu repositorio
4. Configura las variables de entorno:
   ```env
   VITE_API_URL=https://labs-taskmanager-api.hrosario.dev
   CORS_ORIGINS=https://labs-taskmanager-web.hrosario.dev
   DB_PASSWORD=tu_password_seguro
   ```
5. Dokploy detectará automáticamente el `docker-compose.yml`
6. Despliega la aplicación (asegúrate de hacer rebuild completo)
7. Espera 2-3 minutos para que Let's Encrypt emita los certificados SSL

### Verificar Deployment

```bash
# Backend health check
curl https://labs-taskmanager-api.hrosario.dev/health

# Frontend
curl https://labs-taskmanager-web.hrosario.dev/
```

### Troubleshooting

**Error: ERR_SSL_VERSION_OR_CIPHER_MISMATCH**
- Verifica que los registros DNS estén configurados correctamente
- Asegúrate de que Cloudflare SSL/TLS esté en modo "Full (strict)"
- Espera a que Let's Encrypt emita los certificados (2-3 minutos)

**Frontend muestra error de API**
- Verifica que `VITE_API_URL` esté configurado como build argument
- Haz rebuild completo del frontend en Dokploy
- Verifica CORS en el backend

## 📝 Desarrollo Local (sin Docker)

### Backend

```bash
cd backend
npm install
cp .env.example .env
# Configurar la conexión a PostgreSQL en .env
npm run dev
```

### Frontend

```bash
cd frontend
npm install
cp .env.example .env
# Configurar VITE_API_URL en .env
npm run dev
```

## 🤝 Contribuir

Las contribuciones son bienvenidas. Por favor, abre un issue primero para discutir los cambios que te gustaría hacer.

## 📄 Licencia

MIT

---

Hecho con ❤️ para pruebas en VPS con Dokploy
