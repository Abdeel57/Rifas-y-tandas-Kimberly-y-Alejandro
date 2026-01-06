# ✅ Configuración Final - URLs Confirmadas

## 📋 URLs Confirmadas

- **Frontend Netlify**: https://rifas-y-tandas-kimberly-y-alejandro.netlify.app/
- **Backend Railway**: https://rifas-y-tandas-kimberly-y-alejandro-production.up.railway.app

---

## 🔧 Pasos para Completar la Configuración

### Paso 1: Verificar Backend

Abre en tu navegador:
```
https://rifas-y-tandas-kimberly-y-alejandro-production.up.railway.app/api/health
```

**Deberías ver:**
```json
{
  "status": "ok",
  "timestamp": "...",
  "uptime": ...
}
```

Si funciona, continúa. Si no, revisa los logs en Railway.

---

### Paso 2: Configurar Variable en Netlify

1. Ve a Netlify: https://app.netlify.com
2. Abre tu sitio: `rifas-y-tandas-kimberly-y-alejandro`
3. Ve a **Site settings** → **Environment variables**
4. Busca `VITE_API_URL` o haz clic en **Add a variable**
5. Configura:
   - **Key**: `VITE_API_URL`
   - **Value**: `https://rifas-y-tandas-kimberly-y-alejandro-production.up.railway.app/api`
   - ⚠️ **IMPORTANTE**: Debe terminar en `/api`
   - **Scopes**: Selecciona todos (Production, Deploy previews, Branch deploys)
6. Haz clic en **Save**

---

### Paso 3: Agregar CORS en Backend

Ya actualicé el código para agregar el dominio de Netlify a CORS. Ahora necesitas:

1. Hacer commit y push:
   ```bash
   git add backend/src/main.ts
   git commit -m "Agregar dominio Netlify a CORS"
   git push
   ```

2. Railway redeployará automáticamente (espera 2-5 minutos)

---

### Paso 4: Redesplegar Frontend

Después de agregar la variable `VITE_API_URL`:

1. En Netlify → **Deploys**
2. Haz clic en **Trigger deploy** → **Deploy site**
3. Espera a que termine (2-5 minutos)

---

### Paso 5: Probar Login

1. Abre: https://rifas-y-tandas-kimberly-y-alejandro.netlify.app/#/admin
2. Inicia sesión con:
   - **Usuario**: `admin`
   - **Contraseña**: `admin123`

---

## 🔍 Verificación

### En la Consola del Navegador (F12):

Deberías ver:
```
🔌 API Configuration: { 
  API_URL: "https://rifas-y-tandas-kimberly-y-alejandro-production.up.railway.app/api",
  ...
}
```

Si ves errores de CORS, espera a que Railway termine de redeployar.

---

## ✅ Checklist

- [ ] Backend responde en `/api/health`
- [ ] Variable `VITE_API_URL` configurada en Netlify
- [ ] CORS actualizado en `backend/src/main.ts`
- [ ] Cambios pusheados a GitHub
- [ ] Railway redeployado
- [ ] Frontend redesplegado en Netlify
- [ ] Login funciona correctamente

---

**¡Sigue estos pasos y debería funcionar!** 🚀

