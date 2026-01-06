# 🚀 Guía de Configuración - Rifas y tandas Kimberly y Alejandro

## ✅ Cambios Realizados Automáticamente

Ya se han aplicado los siguientes cambios:
- ✅ Nombre del cliente actualizado en `config-cliente.json`
- ✅ Meta tags y título actualizados en `frontend/index.html`
- ✅ Configuración inicial actualizada en `backend/data/settings.json`
- ✅ Comentario agregado en `backend/src/main.ts` para agregar dominios

## 📋 Pasos Siguientes para Completar la Configuración

### Paso 1: Configurar Base de Datos PostgreSQL

**Opción A: Crear en Railway (Recomendado)**
1. Ve a https://railway.app
2. Crea una nueva cuenta o inicia sesión
3. Crea un nuevo proyecto
4. Agrega un servicio PostgreSQL
5. Copia la URL de conexión (DATABASE_URL)

**Opción B: Crear en Supabase**
1. Ve a https://supabase.com
2. Crea una nueva cuenta o inicia sesión
3. Crea un nuevo proyecto
4. Ve a Settings → Database
5. Copia la Connection String (URI)

**Opción C: Usar otra base de datos PostgreSQL**
- Cualquier proveedor de PostgreSQL funciona (Heroku, DigitalOcean, etc.)

### Paso 2: Configurar Variables de Entorno

1. Copia el archivo de ejemplo:
   ```bash
   copy backend\env.example backend\.env
   ```

2. Edita `backend/.env` y completa:
   ```env
   DATABASE_URL="postgresql://usuario:password@host:puerto/database?schema=public"
   JWT_SECRET="genera_un_secret_unico_aqui"
   PORT=3000
   NODE_ENV=development
   ```

   **Para generar un JWT_SECRET seguro, ejecuta:**
   ```bash
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

### Paso 3: Agregar Dominios CORS (Cuando tengas el dominio)

Cuando tengas el dominio del cliente, edita `backend/src/main.ts` y agrega:

```typescript
// Cliente: Rifas y tandas Kimberly y Alejandro
'https://tu-dominio.com',
'https://www.tu-dominio.com',
'http://tu-dominio.com',  // Solo si necesitas HTTP en desarrollo
'http://www.tu-dominio.com',
```

**Si aún no tienes dominio**, puedes continuar con el desarrollo local. Los dominios se pueden agregar después.

### Paso 4: Instalar Dependencias

```bash
npm run install:all
```

Esto instalará las dependencias del proyecto raíz, frontend y backend.

### Paso 5: Ejecutar Migraciones de Base de Datos

```bash
cd backend
npm run migrate:deploy
cd ..
```

Esto creará todas las tablas necesarias en tu base de datos PostgreSQL.

### Paso 6: Crear Usuario Administrador

**Opción A: Usando el script (Recomendado)**
```bash
cd backend
node scripts/create-admin-user.js admin tuPassword123 admin@kimberlyalejandro.com "Administrador"
cd ..
```

**Opción B: Desde el panel web**
1. Inicia la aplicación (Paso 7)
2. Ve a http://localhost:5173/#/admin
3. Si no hay usuarios, el sistema te permitirá crear uno

### Paso 7: Iniciar la Aplicación

```bash
npm start
```

Esto iniciará:
- Frontend en: http://localhost:5173
- Backend en: http://localhost:3000/api

### Paso 8: Configurar desde el Panel de Administración

1. Accede a: http://localhost:5173/#/admin
2. Inicia sesión con las credenciales del Paso 6
3. Ve a **Configuración** y completa:
   - ✅ Nombre del sitio (ya está configurado como "Rifas y tandas Kimberly y Alejandro")
   - 📸 Logo y favicon
   - 🎨 Colores de la marca
   - 📞 Información de contacto (WhatsApp, email, teléfono)
   - 🌐 Redes sociales (Facebook, Instagram, TikTok, YouTube)
   - 💳 Cuentas de pago (bancarias)
   - ❓ Preguntas frecuentes (FAQs)

### Paso 9: Crear tu Primera Rifa

1. En el panel de administración, haz clic en **Rifas**
2. Haz clic en **Nueva Rifa**
3. Completa la información:
   - Nombre de la rifa
   - Descripción
   - Imágenes
   - Precio por boleto
   - Fecha de sorteo
   - Premios
4. Guarda y publica la rifa

## 🔍 Verificación de Errores Comunes

### Error: "CORS blocked"
- **Solución**: Agrega el dominio en `backend/src/main.ts` (Paso 3)
- Reinicia el backend después de hacer cambios

### Error: "Database connection failed"
- **Solución**: Verifica que `DATABASE_URL` en `backend/.env` sea correcta
- Asegúrate de que la base de datos esté accesible desde tu IP
- Verifica las credenciales

### Error: "JWT secret missing"
- **Solución**: Asegúrate de tener `JWT_SECRET` en `backend/.env`
- Genera uno nuevo si es necesario (ver Paso 2)

### Error: "Cannot find module"
- **Solución**: Ejecuta `npm run install:all` (Paso 4)

### Error: "Migration failed"
- **Solución**: Verifica que la base de datos esté vacía o que no haya conflictos
- Asegúrate de que `DATABASE_URL` sea correcta

## 📝 Checklist Final

Antes de considerar la configuración completa, verifica:

- [ ] Base de datos PostgreSQL creada y configurada
- [ ] `backend/.env` configurado con `DATABASE_URL` y `JWT_SECRET`
- [ ] Dominios agregados en `backend/src/main.ts` (si ya tienes dominio)
- [ ] Dependencias instaladas (`npm run install:all`)
- [ ] Migraciones ejecutadas (`npm run migrate:deploy`)
- [ ] Usuario administrador creado
- [ ] Aplicación iniciada y funcionando
- [ ] Panel de administración accesible
- [ ] Configuración básica completada desde el panel
- [ ] Primera rifa creada y publicada

## 🎉 ¡Listo!

Una vez completados estos pasos, tu plataforma estará lista para:
- ✅ Recibir órdenes de clientes
- ✅ Gestionar rifas desde el panel admin
- ✅ Procesar pagos
- ✅ Realizar sorteos
- ✅ Gestionar ganadores

## 📞 Próximos Pasos (Después de la Configuración)

1. **Personalizar diseño**: Configura colores, logo y favicon desde el panel
2. **Agregar contenido**: Crea rifas, FAQs y configura cuentas de pago
3. **Probar flujo completo**: Realiza una compra de prueba
4. **Configurar dominio**: Cuando tengas el dominio, agrégalo en CORS y configura DNS
5. **Desplegar**: Despliega frontend (Netlify/Render) y backend (Railway/Render)

## 🆘 ¿Necesitas Ayuda?

Si encuentras algún error o tienes dudas:
1. Revisa los logs del backend en la consola
2. Verifica que todos los pasos se hayan completado
3. Consulta la documentación en `GUIA-DUPLICACION-CLIENTES.md`

---

**Cliente**: Rifas y tandas Kimberly y Alejandro  
**Fecha de configuración**: $(Get-Date -Format "yyyy-MM-dd")

