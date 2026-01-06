# ⚡ Configurar Railway AHORA - Pasos Rápidos

## 🎯 Tu Situación Actual
Ya creaste el proyecto en Railway y está haciendo deploy, pero necesitas especificar que es el backend.

## ✅ Pasos Inmediatos (5 minutos)

### Paso 1: Configurar Root Directory ⚠️ CRÍTICO

1. Ve a tu proyecto en Railway: https://railway.app
2. Haz clic en el servicio que se está desplegando
3. Ve a la pestaña **Settings** (Configuración)
4. Busca **Root Directory** o **Working Directory**
5. Cambia de `/` (raíz) a: `backend`
6. Haz clic en **Save** o **Update**

**Esto es lo MÁS IMPORTANTE** - sin esto, Railway buscará los archivos en el lugar equivocado.

### Paso 2: Verificar Comandos de Build y Start

Railway debería detectar automáticamente el `railway.json`, pero verifica:

1. En **Settings**, busca **Build Command**
2. Debería ser: `cd backend && npm install && npx prisma generate && npx nest build`
3. Si no está, cópialo y pégalo

4. Busca **Start Command**
5. Debería ser: `cd backend && npm run start:prod`
6. Si no está, cópialo y pégalo

### Paso 3: Agregar Base de Datos PostgreSQL

1. En tu proyecto de Railway, haz clic en **+ New** (arriba a la derecha)
2. Selecciona **Database** → **Add PostgreSQL**
3. Railway creará automáticamente la base de datos
4. **IMPORTANTE**: Railway automáticamente creará la variable `DATABASE_URL` y la conectará a tu servicio backend

### Paso 4: Generar y Agregar JWT_SECRET

1. En tu terminal local, ejecuta:
   ```bash
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```
2. Copia el resultado (será una cadena larga de caracteres)
3. En Railway, ve a tu servicio backend → **Variables**
4. Haz clic en **+ New Variable**
5. Nombre: `JWT_SECRET`
6. Valor: pega el secreto que copiaste
7. Haz clic en **Add**

### Paso 5: Agregar Otras Variables de Entorno

En la misma sección **Variables**, agrega:

1. **NODE_ENV**
   - Nombre: `NODE_ENV`
   - Valor: `production`
   - Click **Add**

2. **PORT** (opcional, Railway lo asigna automáticamente)
   - Nombre: `PORT`
   - Valor: `3000`
   - Click **Add**

### Paso 6: Esperar el Deploy

1. Railway debería reiniciar automáticamente el deploy con la nueva configuración
2. Ve a la pestaña **Deployments** para ver el progreso
3. Espera a que termine (puede tardar 2-5 minutos)

### Paso 7: Ejecutar Migraciones

Una vez que el deploy termine exitosamente:

1. Ve a la pestaña **Deployments**
2. Haz clic en el deployment más reciente
3. Haz clic en los **tres puntos** (⋯) → **Open Shell**
4. En la terminal que se abre, ejecuta:
   ```bash
   cd backend
   npx prisma migrate deploy
   ```
5. Esto creará todas las tablas en la base de datos

### Paso 8: Verificar que Funciona

1. Ve a **Settings** → **Networking**
2. Railway debería haber generado automáticamente un dominio como: `https://tu-backend.up.railway.app`
3. Copia esa URL
4. Abre en tu navegador: `https://tu-backend.up.railway.app/api/health`
5. Deberías ver:
   ```json
   {
     "status": "ok",
     "timestamp": "...",
     "uptime": ...
   }
   ```

### Paso 9: Crear Usuario Administrador

1. En Railway, ve a **Deployments** → **Open Shell** (del último deploy)
2. Ejecuta:
   ```bash
   cd backend
   node scripts/create-admin-user.js admin tuPassword123 admin@kimberlyalejandro.com "Administrador"
   ```
3. Cambia `tuPassword123` por una contraseña segura
4. Cambia el email si quieres

## ✅ Checklist Rápido

- [ ] Root Directory configurado como `backend`
- [ ] Build Command verificado/corregido
- [ ] Start Command verificado/corregido
- [ ] Base de datos PostgreSQL creada
- [ ] Variable `DATABASE_URL` aparece automáticamente (desde PostgreSQL)
- [ ] Variable `JWT_SECRET` agregada
- [ ] Variable `NODE_ENV=production` agregada
- [ ] Deploy completado exitosamente
- [ ] Migraciones ejecutadas
- [ ] Health check funciona (`/api/health`)
- [ ] Usuario administrador creado

## 🆘 Si Algo Sale Mal

### El deploy falla:
1. Ve a **Deployments** → Revisa los **logs**
2. Busca el error específico
3. Verifica que el Root Directory esté en `backend`
4. Verifica que los comandos sean correctos

### No puedo encontrar Root Directory:
- En Railway, ve a **Settings** → Busca **"Root Directory"** o **"Working Directory"**
- Si no aparece, puede estar en una versión diferente de Railway
- Intenta buscar en **"Deploy"** o **"Build"** settings

### La base de datos no se conecta:
- Verifica que ambos servicios (backend y PostgreSQL) estén en el mismo proyecto
- Railway debería conectar automáticamente `DATABASE_URL`
- Si no aparece, ve al servicio PostgreSQL → **Variables** → Copia `DATABASE_URL` → Pégalo en el servicio backend

## 📝 Notas Importantes

1. **Railway detecta automáticamente** el `railway.json` en la raíz del proyecto
2. El **Root Directory** es lo más importante - sin esto, nada funcionará
3. La variable `DATABASE_URL` se conecta automáticamente si PostgreSQL está en el mismo proyecto
4. Después de cambiar el Root Directory, Railway reiniciará el deploy automáticamente

---

**Una vez completados estos pasos, tu backend estará funcionando en Railway.** 🚀

