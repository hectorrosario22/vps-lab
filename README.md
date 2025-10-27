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
- **Frontend**: http://localhost (puerto 80)
- **Backend API**: http://localhost:5000
- **PostgreSQL**: puerto 5432

### 3. Acceder a la aplicación

Abre tu navegador y ve a: http://localhost

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
VITE_API_URL=http://localhost:5000
```

## 🐳 Deployment en Dokploy

Este proyecto está optimizado para deployment en Dokploy:

1. Sube el proyecto a un repositorio Git
2. En Dokploy, crea una nueva aplicación
3. Conecta tu repositorio
4. Dokploy detectará automáticamente el `docker-compose.yml`
5. Despliega la aplicación

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
