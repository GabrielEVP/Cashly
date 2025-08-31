# Configuración de Variables de Entorno - Cashly API

Este documento explica cómo configurar las variables de entorno para la aplicación Cashly API.

## Archivos de Configuración

### `.env.example`
Este archivo contiene un template con todas las variables de entorno necesarias y valores de ejemplo. **Este archivo debe ser incluido en el repositorio** como referencia para otros desarrolladores.

### `.env`
Este archivo contiene los valores reales de las variables de entorno para tu entorno local. **Este archivo NO debe ser incluido en el repositorio** (ya está en `.gitignore`) por seguridad.

## Configuración Inicial

1. **Copia el archivo de ejemplo:**
   ```bash
   cp .env.example .env
   ```

2. **Edita el archivo `.env`** con tus valores reales:
   ```bash
   nano .env
   # o usa tu editor favorito
   ```

## Variables de Entorno Principales

### Base de Datos
```env
DB_HOST=localhost
DB_PORT=3306
DB_NAME=cashly_db
DB_USERNAME=cashly_user
DB_PASSWORD=tu_password_real
```

### CORS y Dominios Permitidos
```env
# Dominios que pueden hacer peticiones a la API
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3001,https://yourdomain.com,https://app.yourdomain.com

# Métodos HTTP permitidos
ALLOWED_METHODS=GET,POST,PUT,DELETE,OPTIONS

# Headers permitidos (* permite todos)
ALLOWED_HEADERS=*

# Permitir credenciales en requests CORS
ALLOW_CREDENTIALS=true
```

### Seguridad JWT
```env
# Clave secreta para JWT (mínimo 256 bits / 32 caracteres)
JWT_SECRET=tu_clave_secreta_super_segura_de_al_menos_32_caracteres

# Tiempo de expiración en milisegundos (86400000 = 24 horas)
JWT_EXPIRATION=86400000
```

### GoCardless (API Bancaria)
```env
GOCARDLESS_SECRET_ID=tu_secret_id
GOCARDLESS_SECRET_KEY=tu_secret_key
GOCARDLESS_ENVIRONMENT=sandbox  # o 'production' para producción
GOCARDLESS_WEBHOOK_ENDPOINT=https://tu-dominio.com/api/v1/webhooks/gocardless
```

### URLs y Webhooks
```env
# URL para webhooks de notificaciones
WEBHOOK_URL=https://tu-webhook-url.com/cashly

# Habilitar notificaciones por email
NOTIFICATION_EMAIL_ENABLED=false

# Habilitar notificaciones por webhook
NOTIFICATION_WEBHOOK_ENABLED=false
```

## Uso en Spring Boot

La aplicación está configurada para leer automáticamente las variables de entorno. Puedes usar el archivo `application-env.properties` que lee las variables con valores por defecto:

```properties
# Ejemplo de cómo se leen las variables:
spring.datasource.url=${DB_URL:jdbc:mysql://localhost:3306/cashly_db}
spring.datasource.username=${DB_USERNAME:default_user}
spring.datasource.password=${DB_PASSWORD}
```

## Configuración por Entornos

### Desarrollo Local
- Usa el archivo `.env` con valores de desarrollo
- Base de datos local MySQL o H2
- URLs localhost para CORS

### Staging/Testing
- Configura las variables en tu plataforma de deploy
- Base de datos de testing
- URLs de staging para CORS

### Producción
- **NUNCA** uses el archivo `.env` en producción
- Configura las variables directamente en el servidor/contenedor
- Usa valores seguros para JWT_SECRET
- Configura dominios reales para CORS

## Variables de Entorno en Docker

Si usas Docker, puedes pasar las variables en el `docker-compose.yml`:

```yaml
version: '3.8'
services:
  cashly-api:
    build: .
    environment:
      - DB_HOST=mysql
      - DB_USERNAME=${DB_USERNAME}
      - DB_PASSWORD=${DB_PASSWORD}
      - JWT_SECRET=${JWT_SECRET}
      - ALLOWED_ORIGINS=https://yourdomain.com
    env_file:
      - .env
```

## Seguridad

### ⚠️ Importantes consideraciones de seguridad:

1. **Nunca** commitees el archivo `.env` al repositorio
2. **Nunca** pongas credenciales reales en `.env.example`
3. Usa una clave JWT_SECRET segura y única para cada entorno
4. Limita ALLOWED_ORIGINS solo a los dominios que necesitas
5. En producción, usa variables de entorno del sistema operativo o secretos del orquestador

### Generar JWT_SECRET seguro:
```bash
# Genera una clave aleatoria de 32 caracteres
openssl rand -hex 32
```

## Validación

Para verificar que las variables se están leyendo correctamente:

1. Ejecuta la aplicación
2. Revisa los logs de inicio
3. Accede a `/actuator/info` (si está habilitado) para ver la configuración
4. Prueba la conexión a la base de datos

## Problemas Comunes

### Error de conexión a base de datos
- Verifica que `DB_HOST`, `DB_PORT`, `DB_USERNAME` y `DB_PASSWORD` sean correctos
- Asegúrate de que la base de datos esté ejecutándose

### Error de CORS
- Verifica que tu dominio frontend esté en `ALLOWED_ORIGINS`
- Asegúrate de incluir el protocolo (http/https) y el puerto si es necesario

### Error de JWT
- Verifica que `JWT_SECRET` tenga al menos 32 caracteres
- No uses caracteres especiales que puedan causar problemas de encoding

## Ejemplo Completo

Aquí tienes un ejemplo de `.env` configurado correctamente:

```env
# Base de datos
DB_HOST=localhost
DB_PORT=3306
DB_NAME=cashly_db
DB_USERNAME=cashly_user
DB_PASSWORD=MiPasswordSuperSeguro123!

# CORS
ALLOWED_ORIGINS=http://localhost:3000,https://app.cashly.com
ALLOWED_METHODS=GET,POST,PUT,DELETE,OPTIONS
ALLOWED_HEADERS=*
ALLOW_CREDENTIALS=true

# JWT
JWT_SECRET=a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6
JWT_EXPIRATION=86400000

# GoCardless
GOCARDLESS_SECRET_ID=live_secretid_abc123
GOCARDLESS_SECRET_KEY=live_secretkey_xyz789
GOCARDLESS_ENVIRONMENT=production
GOCARDLESS_WEBHOOK_ENDPOINT=https://api.cashly.com/api/v1/webhooks/gocardless
```
