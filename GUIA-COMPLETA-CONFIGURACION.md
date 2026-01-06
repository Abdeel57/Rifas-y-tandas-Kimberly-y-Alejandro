# 🚀 Guía Completa de Configuración - Rifas y tandas Kimberly y Alejandro

Esta guía te llevará paso a paso para configurar tanto el backend como el frontend con todas las variables de entorno necesarias.

---

## 📋 Índice

1. [Configurar Backend en Railway](#1-configurar-backend-en-railway)
2. [Configurar Variables de Entorno del Backend](#2-configurar-variables-de-entorno-del-backend)
3. [Ejecutar Migraciones de Base de Datos](#3-ejecutar-migraciones-de-base-de-datos)
4. [Configurar Frontend en Netlify](#4-configurar-frontend-en-netlify)
5. [Configurar Variables de Entorno del Frontend](#5-configurar-variables-de-entorno-del-frontend)
6. [Verificar que Todo Funciona](#6-verificar-que-todo-funciona)

---

## 1. Configurar Backend en Railway

### Paso 1.1: Crear el Servicio Backend

1. Ve a https://railway.app
2. Crea un nuevo proyecto o selecciona uno existente
3. Haz clic en **+ New** → **GitHub Repo**
4. Conecta tu repositorio: `Abdeel57/Rifas-y-tandas-Kimberly-y-Alejandro`
5. Railway comenzará a hacer deploy automáticamente

### Paso 1.2: Configurar Root Directory

⚠️ **CRÍTICO**: Debes especificar que es el backend

1. Haz clic en el servicio que se está desplegando
2. Ve a **Settings** (Configuración)
3. Busca **Root Directory** o **Working Directory**
4. Cambia de `/` (raíz) a: `backend`
5. Haz clic en **Save**

### Paso 1.3: Verificar Comandos de Build y Start

Railway debería detectar automáticamente el `railway.json`, pero verifica:

**Build Command:**
```
cd backend && npm install && npx prisma generate && npx nest build
```

**Start Command:**
```
cd backend && npm run start:prod
```

Si no están configurados automáticamente, agrégalos en **Settings** → **Deploy**.

---

## 2. Configurar Variables de Entorno del Backend

### Paso 2.1: Crear Base de Datos PostgreSQL

1. En tu proyecto de Railway, haz clic en **+ New**
2. Selecciona **Database** → **Add PostgreSQL**
3. Railway creará automáticamente la base de datos
4. **IMPORTANTE**: Railway automáticamente creará la variable `DATABASE_URL` y la conectará a tu servicio backend

### Paso 2.2: Generar JWT_SECRET

En tu terminal local, ejecuta:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Copia el resultado (será una cadena larga de caracteres hexadecimales).

### Paso 2.3: Agregar Variables de Entorno en Railway

En Railway, ve a tu servicio backend → **Variables** → **+ New Variable**

Agrega las siguientes variables:

#### Variables Requeridas:

1. **DATABASE_URL** (ya debería estar automáticamente desde PostgreSQL)
   - Si no aparece, ve al servicio PostgreSQL → **Variables** → Copia `DATABASE_URL` → Pégalo en el servicio backend

2. **JWT_SECRET**
   - Nombre: `JWT_SECRET`
   - Valor: pega el secreto que generaste en el Paso 2.2
   - Ejemplo: `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6`

3. **NODE_ENV**
   - Nombre: `NODE_ENV`
   - Valor: `production`

#### Variables Opcionales:

4. **PORT** (Railway lo asigna automáticamente, pero puedes dejarlo)
   - Nombre: `PORT`
   - Valor: `3000`

### Paso 2.4: Verificar Variables

Tu lista de variables debería verse así:

```
DATABASE_URL=postgresql://postgres:****@host:port/railway (automática)
JWT_SECRET=tu_secret_generado
NODE_ENV=production
PORT=3000
```

---

## 3. Ejecutar Migraciones de Base de Datos

### Paso 3.1: Esperar el Deploy

Espera a que Railway termine de desplegar el backend (puede tardar 2-5 minutos).

### Paso 3.2: Ejecutar Migraciones desde Railway

1. En Railway, ve a tu servicio backend
2. Haz clic en la pestaña **Deployments**
3. Haz clic en el deployment más reciente
4. Haz clic en los **tres puntos** (⋯) → **Open Shell**
5. En la terminal que se abre, ejecuta:

```bash
cd backend
npx prisma migrate deploy
```

Esto creará todas las tablas necesarias en tu base de datos PostgreSQL.

### Paso 3.3: Verificar Migraciones

Deberías ver un mensaje como:
```
✅ Applied migration: 20250924231524_init
✅ Applied migration: 20250117000000_add_text_color_fields
✅ Applied migration: 20251017150400_add_released_status
```

### Paso 3.4: Crear Usuario Administrador

En la misma terminal de Railway, ejecuta:

```bash
cd backend
node scripts/create-admin-user.js admin tuPassword123 admin@kimberlyalejandro.com "Administrador"
```

**Cambia:**
- `admin` → tu nombre de usuario deseado
- `tuPassword123` → una contraseña segura
- `admin@kimberlyalejandro.com` → tu email
- `"Administrador"` → el nombre completo del administrador

---

## 4. Configurar Frontend en Netlify

### Paso 4.1: Conectar Repositorio

1. Ve a https://app.netlify.com
2. Haz clic en **Add new site** → **Import an existing project**
3. Conecta con GitHub
4. Selecciona el repositorio: `Abdeel57/Rifas-y-tandas-Kimberly-y-Alejandro`

### Paso 4.2: Configurar Build Settings

En la configuración del sitio:

- **Base directory**: `frontend`
- **Build command**: `npm run build`
- **Publish directory**: `frontend/dist`

O si Netlify detecta automáticamente:
- Verifica que el **Base directory** sea `frontend`
- Verifica que el **Publish directory** sea `frontend/dist`

---

## 5. Configurar Variables de Entorno del Frontend

### Paso 5.1: Obtener URL del Backend

1. En Railway, ve a tu servicio backend
2. Ve a **Settings** → **Networking**
3. Railway debería haber generado automáticamente un dominio como:
   ```
   https://tu-backend.up.railway.app
   ```
4. **Copia esta URL completa**

### Paso 5.2: Agregar Variable en Netlify

1. En Netlify, ve a tu sitio
2. Ve a **Site settings** → **Environment variables**
3. Haz clic en **Add a variable**
4. Agrega:
   - **Key**: `VITE_API_URL`
   - **Value**: `https://tu-backend.up.railway.app/api`
   - **Scopes**: Selecciona todos los ambientes (Production, Deploy previews, Branch deploys)
5. Haz clic en **Save**

### Paso 5.3: Configurar Redirects (Opcional)

Si quieres que las rutas `/api/*` redirijan al backend:

1. En Netlify, ve a **Site settings** → **Redirects and rewrites**
2. Agrega una nueva regla:
   - **From**: `/api/*`
   - **To**: `https://tu-backend.up.railway.app/api/:splat`
   - **Status**: `200`

O edita `frontend/netlify.toml` y descomenta la sección de redirects con tu URL.

### Paso 5.4: Redesplegar Frontend

1. En Netlify, ve a **Deploys**
2. Haz clic en **Trigger deploy** → **Deploy site**
3. Esto reconstruirá el sitio con las nuevas variables de entorno

---

## 6. Verificar que Todo Funciona

### Paso 6.1: Verificar Backend

1. Abre en tu navegador: `https://tu-backend.up.railway.app/api/health`
2. Deberías ver:
   ```json
   {
     "status": "ok",
     "timestamp": "...",
     "uptime": ...
   }
   ```

### Paso 6.2: Verificar Frontend

1. Abre tu sitio de Netlify (ej: `https://tu-sitio.netlify.app`)
2. Debería cargar correctamente
3. Ve a la consola del navegador (F12) y verifica que no haya errores de conexión con el backend

### Paso 6.3: Probar Login Admin

1. Ve a: `https://tu-sitio.netlify.app/#/admin`
2. Inicia sesión con las credenciales que creaste en el Paso 3.4
3. Deberías poder acceder al panel de administración

---

## 🔧 Configuración Adicional

### Agregar Dominios CORS

Cuando tengas el dominio del frontend:

1. Edita `backend/src/main.ts`
2. Busca la sección `// Cliente: Rifas y tandas Kimberly y Alejandro`
3. Descomenta y agrega tus dominios:
   ```typescript
   // Cliente: Rifas y tandas Kimberly y Alejandro
   'https://tu-dominio.com',
   'https://www.tu-dominio.com',
   'http://tu-dominio.com',  // Solo si necesitas HTTP en desarrollo
   'http://www.tu-dominio.com',
   ```
4. Haz commit y push a GitHub
5. Railway redeployará automáticamente

### Configurar Dominio Personalizado en Railway

1. En Railway, ve a tu servicio backend → **Settings** → **Networking**
2. Haz clic en **Custom Domain**
3. Agrega tu dominio personalizado
4. Configura los registros DNS según las instrucciones de Railway

### Configurar Dominio Personalizado en Netlify

1. En Netlify, ve a **Site settings** → **Domain management**
2. Haz clic en **Add custom domain**
3. Sigue las instrucciones para configurar DNS

---

## ✅ Checklist Final

### Backend:
- [ ] Servicio creado en Railway
- [ ] Root Directory configurado como `backend`
- [ ] Base de datos PostgreSQL creada
- [ ] Variable `DATABASE_URL` configurada (automática)
- [ ] Variable `JWT_SECRET` generada y configurada
- [ ] Variable `NODE_ENV=production` configurada
- [ ] Migraciones ejecutadas exitosamente
- [ ] Usuario administrador creado
- [ ] Health check funciona (`/api/health`)
- [ ] Dominio público generado

### Frontend:
- [ ] Sitio creado en Netlify
- [ ] Repositorio conectado
- [ ] Base directory configurado como `frontend`
- [ ] Variable `VITE_API_URL` configurada con URL del backend
- [ ] Deploy exitoso
- [ ] Sitio carga correctamente
- [ ] Login admin funciona

---

## 🆘 Solución de Problemas

### Error: "Cannot find module" en Railway
- **Solución**: Verifica que el Root Directory esté configurado como `backend`

### Error: "Database connection failed"
- **Solución**: Verifica que `DATABASE_URL` esté correcta y que la base de datos esté activa

### Error: "CORS blocked" en el frontend
- **Solución**: Agrega el dominio de Netlify en `backend/src/main.ts` en la sección de CORS

### El frontend no se conecta al backend
- **Solución**: Verifica que `VITE_API_URL` en Netlify sea correcta (debe terminar en `/api`)

### Las migraciones fallan
- **Solución**: Asegúrate de que la base de datos esté vacía o que no haya conflictos

---

## 📝 Resumen de URLs Importantes

- **Railway Dashboard**: https://railway.app/dashboard
- **Netlify Dashboard**: https://app.netlify.com
- **Backend Health Check**: `https://tu-backend.up.railway.app/api/health`
- **Frontend**: `https://tu-sitio.netlify.app`
- **Panel Admin**: `https://tu-sitio.netlify.app/#/admin`

---

**¡Listo! Tu plataforma debería estar completamente configurada y funcionando.** 🎉

