# 🔧 Solución: Railway sin opción Root Directory

Si no encuentras la opción "Root Directory" en Railway, **no te preocupes**. Hay varias formas de solucionarlo:

## ✅ Solución 1: Usar railway.json (Ya configurado)

El archivo `railway.json` ya tiene los comandos con `cd backend`, lo que significa que **debería funcionar** aunque no veas la opción Root Directory.

**Verifica que Railway esté usando el archivo:**
1. En Railway, ve a **Settings** → **Deploy**
2. Verifica que los comandos sean:
   - **Build**: `cd backend && npm install && npx prisma generate && npx nest build`
   - **Start**: `cd backend && npm run start:prod`

Si estos comandos están correctos, **ya está funcionando** porque el `cd backend` cambia al directorio correcto.

## ✅ Solución 2: Buscar en otros lugares

La opción Root Directory puede estar en:

### Opción A: Settings → General
1. Ve a tu servicio en Railway
2. Haz clic en **Settings**
3. Busca la pestaña **General** (no Deploy)
4. Busca **"Source"** o **"Root Directory"**

### Opción B: Settings → Source
1. Ve a **Settings**
2. Busca una sección llamada **"Source"**
3. Ahí puede estar **"Root Directory"** o **"Working Directory"**

### Opción C: En la configuración del servicio
1. Haz clic en el nombre del servicio (arriba)
2. Puede haber opciones adicionales ahí

## ✅ Solución 3: Crear archivo nixpacks.toml

Si Railway usa Nixpacks, puedes crear un archivo `nixpacks.toml` en la raíz del proyecto:

```toml
[phases.setup]
nixPkgs = ['nodejs-18_x']

[phases.install]
cmds = ['cd backend && npm install']

[phases.build]
cmds = ['cd backend && npx prisma generate && npm run build']

[start]
cmd = 'cd backend && npm run start:prod'
```

## ✅ Solución 4: Verificar que funciona sin Root Directory

**La buena noticia**: Si los comandos Build y Start ya tienen `cd backend`, **debería funcionar sin configurar Root Directory**.

### Cómo verificar:

1. **Revisa los logs del deploy:**
   - Ve a **Deployments** → Último deploy → **View Logs**
   - Busca líneas como:
     ```
     Running: cd backend && npm install
     ```
   - Si ves que ejecuta `cd backend`, está funcionando

2. **Si el deploy falla con "Cannot find module":**
   - Significa que Railway no está cambiando al directorio backend
   - En ese caso, necesitas encontrar la opción Root Directory o usar otra solución

## 🎯 Lo Más Importante

**Lo que SÍ necesitas verificar:**

1. ✅ **Build Command** debe tener `cd backend`:
   ```
   cd backend && npm install && npx prisma generate && npx nest build
   ```

2. ✅ **Start Command** debe tener `cd backend`:
   ```
   cd backend && npm run start:prod
   ```

3. ✅ **Variables de entorno** configuradas:
   - `DATABASE_URL` (automática desde PostgreSQL)
   - `JWT_SECRET`
   - `NODE_ENV=production`

## 🆘 Si el Deploy Falla

Si el deploy falla con errores como "Cannot find module" o "package.json not found":

1. **Verifica los logs** para ver dónde está buscando los archivos
2. **Intenta agregar un archivo `.railwayignore`** en la raíz (no necesario, pero puede ayudar)
3. **Contacta soporte de Railway** o busca en su documentación sobre cómo configurar Root Directory en tu versión

## 📝 Nota sobre railway.json

He actualizado el `railway.json` para incluir `"rootDirectory": "backend"`. Esto puede ayudar a Railway a detectar automáticamente el directorio correcto.

**Haz commit y push de este cambio:**
```bash
git add railway.json
git commit -m "Agregar rootDirectory a railway.json"
git push
```

Railway debería detectar el cambio y redeployar automáticamente.

---

**En resumen**: Si los comandos Build y Start ya tienen `cd backend`, debería funcionar. Si no funciona, busca Root Directory en Settings → General o Source.

