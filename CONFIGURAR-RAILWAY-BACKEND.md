# 🚂 Configurar Backend en Railway - Rifas y tandas Kimberly y Alejandro

## 📋 Pasos para Configurar el Backend en Railway

### Paso 1: Verificar el Servicio en Railway

1. Ve a tu proyecto en Railway: https://railway.app
2. Si ya creaste el servicio pero no está configurado correctamente:
   - Haz clic en el servicio
   - Ve a **Settings** (Configuración)

### Paso 2: Configurar el Directorio Raíz

En la configuración del servicio:

1. Busca la sección **Root Directory** o **Working Directory**
2. Establece el directorio raíz como: `backend`
   - Esto le dice a Railway que el código del backend está en la carpeta `backend/`

### Paso 3: Verificar Comandos de Build y Start

Railway debería detectar automáticamente el `railway.json`, pero verifica:

**Build Command:**
```bash
cd backend && npm install && npx prisma generate && npx nest build
```

**Start Command:**
```bash
cd backend && npm run start:prod
```

O si Railway no detecta el railway.json, configura manualmente:
- **Build**: `cd backend && npm install && npx prisma generate && npx nest build`
- **Start**: `cd backend && npm run start:prod`

### Paso 4: Agregar Base de Datos PostgreSQL

1. En tu proyecto de Railway, haz clic en **+ New**
2. Selecciona **Database** → **Add PostgreSQL**
3. Railway creará automáticamente una base de datos PostgreSQL
4. **IMPORTANTE**: Copia la variable `DATABASE_URL` que Railway genera automáticamente

### Paso 5: Configurar Variables de Entorno

En la configuración del servicio del backend, ve a **Variables** y agrega:

#### Variables Requeridas:

```env
DATABASE_URL=postgresql://postgres:password@host:port/railway
```

**Nota**: Railway genera automáticamente `DATABASE_URL` cuando agregas PostgreSQL. Solo necesitas:
1. Ir a la pestaña **Variables** del servicio backend
2. Railway debería mostrar automáticamente `DATABASE_URL` desde el servicio PostgreSQL
3. Si no aparece automáticamente, puedes agregarla manualmente desde el servicio PostgreSQL:
   - Ve al servicio PostgreSQL
   - Haz clic en **Variables**
   - Copia `DATABASE_URL`
   - Pégalo en las variables del servicio backend

#### Generar JWT_SECRET:

```bash
# En tu terminal local, ejecuta:
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Luego agrega la variable:
```env
JWT_SECRET=tu_secret_generado_aqui
```

#### Otras Variables Recomendadas:

```env
NODE_ENV=production
PORT=3000
```

### Paso 6: Configurar CORS (Opcional pero Recomendado)

Si ya tienes el dominio del frontend, agrega:

```env
CLIENT_DOMAIN=tu-dominio.com
```

Y asegúrate de que los dominios estén en `backend/src/main.ts` (ya está configurado para agregar dominios).

### Paso 7: Ejecutar Migraciones

Railway puede ejecutar migraciones automáticamente durante el build. Verifica que en `railway.json` o en las variables de entorno tengas configurado:

**Opción A: Agregar script de post-deploy**

Puedes crear un script que ejecute las migraciones después del deploy. Agrega esto en las variables de Railway:

```env
RAILWAY_RUN_MIGRATIONS=true
```

O modifica el build command para incluir migraciones:

```bash
cd backend && npm install && npx prisma migrate deploy && npx prisma generate && npx nest build
```

**Opción B: Ejecutar migraciones manualmente después del primer deploy**

1. Una vez que el servicio esté desplegado
2. Ve a la pestaña **Deployments**
3. Haz clic en los tres puntos del deployment más reciente
4. Selecciona **Open Shell**
5. Ejecuta:
   ```bash
   cd backend
   npx prisma migrate deploy
   ```

### Paso 8: Verificar el Deploy

1. Una vez desplegado, Railway te dará una URL como: `https://tu-backend.railway.app`
2. Verifica que funcione visitando: `https://tu-backend.railway.app/api/health`
3. Deberías ver una respuesta JSON con `status: "ok"`

### Paso 9: Crear Usuario Administrador

Después de que las migraciones estén ejecutadas:

1. Ve a la terminal de Railway (Open Shell)
2. Ejecuta:
   ```bash
   cd backend
   node scripts/create-admin-user.js admin tuPassword123 admin@kimberlyalejandro.com "Administrador"
   ```

O espera a configurar desde el panel web cuando el frontend esté desplegado.

## 🔧 Configuración Avanzada

### Si Railway no detecta el railway.json:

1. Ve a **Settings** del servicio
2. En **Build Command**, agrega:
   ```
   cd backend && npm install && npx prisma generate && npx nest build
   ```
3. En **Start Command**, agrega:
   ```
   cd backend && npm run start:prod
   ```

### Configurar Dominio Personalizado (Opcional)

1. En Railway, ve a **Settings** → **Networking**
2. Haz clic en **Generate Domain** o **Add Custom Domain**
3. Copia el dominio generado
4. Agrega este dominio en `backend/src/main.ts` en la sección de CORS

## ✅ Checklist de Configuración

- [ ] Servicio creado en Railway
- [ ] Root Directory configurado como `backend`
- [ ] Build Command configurado correctamente
- [ ] Start Command configurado correctamente
- [ ] Base de datos PostgreSQL agregada
- [ ] Variable `DATABASE_URL` configurada (automática desde PostgreSQL)
- [ ] Variable `JWT_SECRET` generada y configurada
- [ ] Variable `NODE_ENV=production` configurada
- [ ] Migraciones ejecutadas
- [ ] Deploy exitoso
- [ ] Health check funcionando (`/api/health`)
- [ ] Usuario administrador creado

## 🆘 Solución de Problemas

### Error: "Cannot find module"
- **Solución**: Verifica que el Root Directory esté configurado como `backend`

### Error: "Prisma Client not generated"
- **Solución**: Asegúrate de que el build command incluya `npx prisma generate`

### Error: "Database connection failed"
- **Solución**: Verifica que `DATABASE_URL` esté correctamente configurada y que el servicio PostgreSQL esté conectado

### Error: "Migration failed"
- **Solución**: Ejecuta las migraciones manualmente desde la terminal de Railway

### El servicio no inicia
- **Solución**: Verifica los logs en Railway para ver el error específico
- Asegúrate de que el Start Command sea correcto

## 📝 Notas Importantes

1. **Railway detecta automáticamente** el `railway.json` en la raíz del proyecto
2. Si tienes múltiples servicios (frontend y backend), crea **dos servicios separados**:
   - Uno para backend (Root Directory: `backend`)
   - Uno para frontend (Root Directory: `frontend`)
3. La variable `DATABASE_URL` se conecta automáticamente si ambos servicios están en el mismo proyecto de Railway

---

**Una vez configurado, tu backend estará disponible en una URL de Railway y podrás conectarlo con tu frontend.**

