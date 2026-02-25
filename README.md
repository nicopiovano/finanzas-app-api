# 🔐 Login — Backend (API Laravel)

API de autenticación con **Laravel 12** y **Sanctum**: registro, login, logout y sesión con cookies para SPA.

---

## 🛠 Stack

| Tecnología | Uso |
|------------|-----|
| **Laravel 12** | Framework PHP |
| **Laravel Sanctum** | Auth con tokens / cookies para SPA |
| **MySQL 8** | Base de datos |
| **Vite** | Assets (CSS/JS) del backend |
| **PHP 8.2+** | Runtime |

---

## 📋 Requisitos

- **PHP** 8.2 o superior
- **Composer**
- **Node.js** 18+ (para Vite)
- **MySQL** 8 (o usar Docker)

---

## 🚀 Opción A: Levantar con Docker (recomendado)

Desde la carpeta **login** (donde está `docker-compose.yml`):

```bash
cd login
docker compose up -d
```

- **API:** http://localhost:8001  
- **MySQL:** puerto 3306 (usuario `logueo`, contraseña `secret`, DB `logueo`)

El contenedor ejecuta migraciones al iniciar. No hace falta correr `php artisan serve` a mano.

---

## 🚀 Opción B: Levantar en local (sin Docker)

### 1. Base de datos

Crea una base de datos MySQL llamada `logueo` (o la que uses en `.env`).

### 2. Instalar dependencias PHP

```bash
cd login/backend
composer install
```

### 3. Configurar entorno

```bash
cp .env.example .env
php artisan key:generate
```

Edita `.env` y configura la conexión a MySQL:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=logueo
DB_USERNAME=tu_usuario
DB_PASSWORD=tu_password
```

### 4. Migraciones

```bash
php artisan migrate
```

### 5. Servidor + Vite (desarrollo)

**Opción rápida (todo en uno):**

```bash
composer run dev
```

Esto levanta `php artisan serve`, cola, logs y Vite.

**O por separado:**

Terminal 1 — API:

```bash
php artisan serve --port=8001
```

Terminal 2 — Vite (assets):

```bash
npm install
npm run dev
```

La API quedará en **http://localhost:8001**.

---

## 📡 Endpoints principales

| Método | Ruta | Descripción |
|--------|------|-------------|
| `GET` | `/api/health` | Health check |
| `GET` | `/api/email-exists?email=...` | Comprueba si el email existe |
| `POST` | `/api/register` | Registro (body: name, email, password) |
| `POST` | `/api/login` | Login (body: email, password) |
| `GET` | `/api/user` | Usuario autenticado (requiere cookie/token) |
| `POST` | `/api/logout` | Cerrar sesión |

El frontend debe usar **Sanctum** con cookies (origen permitido en CORS y `SANCTUM_STATEFUL_DOMAINS`).

---

## 🔧 Variables de entorno importantes

Para que el frontend (por ejemplo en `localhost:5173`) pueda autenticarse:

```env
APP_URL=http://localhost:8001
SANCTUM_STATEFUL_DOMAINS=localhost:5173,127.0.0.1:5173
CORS_ALLOWED_ORIGINS=http://localhost:5173
SESSION_DOMAIN=localhost
SESSION_SECURE_COOKIE=false
```

---

## 📜 Scripts útiles

| Comando | Descripción |
|---------|-------------|
| `composer run setup` | install, .env, key, migrate, npm install, npm run build |
| `composer run dev` | serve + queue + pail + vite en una sola terminal |
| `composer run test` | Ejecuta los tests de PHP |

---

## 📁 Estructura relevante

```
backend/
├── app/Http/Controllers/AuthController.php
├── routes/api.php
├── config/sanctum.php
├── config/cors.php
├── database/migrations/
└── .env
```

---

## 💡 Notas

- Con **Docker**, la API está en **http://localhost:8001** y el frontend debe apuntar a esa URL.
- En local, usa `php artisan serve --port=8001` para no chocar con otros proyectos.
- Para producción, configura CORS y `SANCTUM_STATEFUL_DOMAINS` con el dominio real del frontend.
# finanzas-app
