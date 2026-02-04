# 🌐 Configurar Dominio Personalizado: rifasytandaskya.com

## ✅ Cambios Realizados en el Código

Ya actualicé el código para usar tu nuevo dominio:

1. ✅ **CORS actualizado** en `backend/src/main.ts` - Agregado `rifasytandaskya.com`
2. ✅ **Meta tags actualizados** en `frontend/index.html` - URLs actualizadas
3. ✅ **Config cliente actualizado** en `config-cliente.json`

---

## 🚀 Pasos para Configurar el Dominio en Netlify

### Paso 1: Configurar Dominio en Netlify

1. Ve a Netlify: https://app.netlify.com
2. Abre tu sitio: `rifas-y-tandas-kimberly-y-alejandro`
3. Ve a **Site settings** → **Domain management**
4. Haz clic en **Add custom domain**
5. Ingresa: `rifasytandaskya.com`
6. Haz clic en **Verify**

### Paso 2: Configurar DNS

Netlify te dará instrucciones para configurar DNS. Típicamente necesitas:

#### Opción A: Configurar DNS en tu Proveedor de Dominio

Ve a donde compraste el dominio (GoDaddy, Namecheap, etc.) y configura:

**Registros DNS necesarios:**

1. **Registro A o CNAME** (Netlify te dirá cuál):
   - **Tipo**: CNAME (recomendado) o A
   - **Nombre**: `@` o `rifasytandaskya.com`
   - **Valor**: `rifas-y-tandas-kimberly-y-alejandro.netlify.app` (o la IP que Netlify te dé)

2. **Registro CNAME para www**:
   - **Tipo**: CNAME
   - **Nombre**: `www`
   - **Valor**: `rifas-y-tandas-kimberly-y-alejandro.netlify.app`

#### Opción B: Usar Nameservers de Netlify (Más Fácil)

1. En Netlify, después de agregar el dominio, te dará nameservers
2. Ve a tu proveedor de dominio
3. Cambia los nameservers a los que Netlify te proporcionó
4. Espera 24-48 horas para que se propague

### Paso 3: Esperar Propagación DNS

- Puede tardar desde minutos hasta 48 horas
- Netlify te mostrará el estado de verificación

### Paso 4: Verificar SSL

Netlify automáticamente generará un certificado SSL gratuito para tu dominio. Esto puede tardar unos minutos después de que DNS se propague.

---

## 🔧 Configuración Adicional

### Verificar que CORS Esté Actualizado

Ya actualicé el código, pero verifica:

1. `backend/src/main.ts` debe tener:
   ```typescript
   'https://rifasytandaskya.com',
   'https://www.rifasytandaskya.com',
   ```

2. Haz commit y push si aún no lo has hecho:
   ```bash
   git add backend/src/main.ts frontend/index.html config-cliente.json
   git commit -m "Actualizar dominio a rifasytandaskya.com"
   git push
   ```

3. Railway redeployará automáticamente

### Verificar Variable VITE_API_URL en Netlify

Asegúrate de que en Netlify → Environment variables tengas:
- **Key**: `VITE_API_URL`
- **Value**: `https://rifas-y-tandas-kimberly-y-alejandro-production.up.railway.app/api`

---

## ✅ Verificación Final

Después de configurar el dominio:

1. **Espera a que DNS se propague** (puede tardar hasta 48 horas)
2. **Verifica que el sitio carga**: `https://rifasytandaskya.com`
3. **Verifica SSL**: Debe tener el candado verde 🔒
4. **Prueba el login**: `https://rifasytandaskya.com/#/admin`
5. **Verifica CORS**: No debe haber errores de CORS en la consola

---

## 🆘 Solución de Problemas

### El dominio no carga

**Solución:**
- Verifica que DNS esté configurado correctamente
- Espera más tiempo para la propagación DNS
- Verifica en Netlify el estado del dominio

### Error de CORS después de cambiar dominio

**Solución:**
- Verifica que Railway haya redeployado después del cambio
- Verifica que el dominio esté en `backend/src/main.ts`
- Espera 2-5 minutos después del push

### SSL no funciona

**Solución:**
- Espera a que Netlify genere el certificado (puede tardar minutos)
- Verifica que DNS esté correctamente configurado
- Netlify genera SSL automáticamente, solo espera

---

## 📋 Checklist

- [ ] Dominio agregado en Netlify
- [ ] DNS configurado en proveedor de dominio
- [ ] CORS actualizado en `backend/src/main.ts` (ya hecho)
- [ ] Meta tags actualizados en `frontend/index.html` (ya hecho)
- [ ] Cambios pusheados a GitHub
- [ ] Railway redeployado
- [ ] DNS propagado (verificado en Netlify)
- [ ] SSL activo (verificado en Netlify)
- [ ] Sitio carga en `https://rifasytandaskya.com`
- [ ] Login funciona correctamente

---

**¡Una vez que DNS se propague, tu sitio estará disponible en https://rifasytandaskya.com!** 🎉

