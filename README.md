# 🗺️ Dawarich Docker - Location History Tracker Autohospedado

[![GitHub Stars](https://img.shields.io/github/stars/Freika/dawarich?style=for-the-badge&logo=github&label=Stars&color=yellow)](https://github.com/Freika/dawarich)
[![Docker Pulls](https://img.shields.io/docker/pulls/freika/dawarich?style=for-the-badge&logo=docker&label=Docker%20Pulls&color=blue)](https://hub.docker.com/r/freika/dawarich)
[![License](https://img.shields.io/github/license/Freika/dawarich?style=for-the-badge&label=License&color=red)](https://github.com/Freika/dawarich/blob/main/LICENSE)
[![GitHub Release](https://img.shields.io/github/v/release/Freika/dawarich?style=for-the-badge&logo=github&label=Release&color=green)](https://github.com/Freika/dawarich/releases)

## 📋 Descripción general

**Dawarich** es un *location history tracker* autohospedado basado en **Ruby on Rails** que proporciona una alternativa completa a **Google Timeline**. Visualiza tu historial de ubicaciones en un mapa interactivo, realiza seguimiento en vivo con apps móviles (iOS, Android, OwnTracks, Overland, GPSLogger), crea viajes, comparte ubicación con familiares, integra fotos georreferenciadas (Immich, Photoprism), importa datos de Google Takeout, GPX, GeoJSON y genera estadísticas detalladas de tus desplazamientos.

> **10.1k+ ⭐ en GitHub** | **AGPL-3.0 Open Source** | **Ruby on Rails + PostgreSQL** | **Production-ready Docker**

## ✨ Características principales

- 📍 **Live location tracking** — iOS, Android, OwnTracks, Overland, GPSLogger, PhoneTrack, Home Assistant
- 🗺️ **Mapa interactivo** — Heatmap, puntos, líneas, capas FOW (fog of war), visualización personalizable
- 📊 **Timeline + Insights** — Historial por día, insights automáticos ("Most visited place is home")
- 👨‍👩‍👧‍👦 **Family sharing** — Comparte ubicación con familiares, consentimiento mutuo
- ✈️ **Viajes (Trips)** — Crea viajes por rango de fechas, ruta, distancia, tiempo, notas, fotos
- 📈 **Estadísticas** — Países/ciudades visitadas, distancia, tiempo, por mes/año, tax residency (días por país)
- 📸 **Integración fotos** — Immich, Photoprism sync, visualiza fotos geotagged en mapa y viajes
- 📥 **Import multi-fuente** — Google Takeout, OwnTracks, Strava, Immich, GPX/GeoJSON, EXIF photos
- 📤 **Export data** — GeoJSON, GPX, plena portabilidad y ownership de datos
- 🔒 **Privacy-first** — Sin cloud lock-in, autohospedado, open source, control total de datos
- 🐳 **Production-ready Docker** — Ruby on Rails, PostgreSQL, Redis, Sidekiq, actualizaciones frecuentes

## 📋 Requisitos del sistema

- **Docker & Docker Compose v2+**
- **RAM**: 2 GB – 4 GB mínimo (Ruby app + PostgreSQL + Redis)
- **Disco**: 20 GB – 100+ GB (según volumen de historial de ubicaciones)
- **Puerto TCP**: 3000 (Web UI)
- **PostgreSQL 13+** (incluido en compose)
- **Redis** (para jobs y caché, incluido en compose)
- **Ruby 3.2+** (incluido en imagen Docker)
- **Node.js + npm** (compilación assets, incluido en imagen)
- **Opcional**: Reverse proxy (nginx/Apache + HTTPS)
- **Opcional**: Servidor Immich/Photoprism (integración fotos)

> ⚠️ **Bajo desarrollo activo**: Dawarich actualiza frecuentemente. Lee *release notes* antes de actualizar. Posibles *breaking changes*. **NO actualices automático**.  
> 💾 **Backup crítico**: Siempre backup antes de actualizar. Datos de ubicación son irreemplazables. Usa `pg_dump` de PostgreSQL.

## 🐳 Instalación

### Paso 1: Clonar repositorio
```bash
git clone https://github.com/Freika/dawarich.git
cd dawarich
```

### Paso 2: Configurar `.env`
```bash
# Copiar plantilla
cp .env.example .env

# Editar variables críticas
nano .env
```

**Variables mínimas a cambiar en `.env`:**
```env
# Generar con: openssl rand -hex 32
SECRET_KEY_BASE=tu_clave_secreta_aqui

# Tu dominio o IP local
HOSTNAME=localhost

# Opcional: configuración email, S3, etc.
```

### Paso 3: Generar `SECRET_KEY_BASE`
```bash
openssl rand -hex 32
# Copiar salida y pegar en .env como SECRET_KEY_BASE
```

### Paso 4: Iniciar Dawarich
```bash
# Desde directorio dawarich
docker compose -f docker/docker-compose.yml up -d

# Esperar ~30 segundos para migraciones
docker compose -f docker/docker-compose.yml logs -f
# Debería aparecer "Started rails app" cuando esté listo
```

### Acceder a Dawarich
🗺️ **Web UI**: `http://localhost:3000`  
💡 **Desde otros dispositivos**: `http://<IP_SERVIDOR>:3000` (obtén IP con `hostname -I`)

## ⚙️ Configuración

1. **Variables de entorno** — Edita `.env` con `SECRET_KEY_BASE`, `HOSTNAME`, credenciales email/S3 si aplica
2. **Reverse proxy (recomendado producción)** — Configura nginx/Apache con HTTPS apuntando a puerto 3000
3. **Integración Immich/Photoprism** — En UI: Settings → Integrations → añade URL + credenciales
4. **Apps móviles** — Configura Server URL en apps iOS/Android/OwnTracks apuntando a tu instancia
5. **Backup automático** — Programa `pg_dump` periódico de la BD PostgreSQL

## 🚀 Primeros pasos

1. **Cambiar credenciales por defecto**  
   Login: `demo@dawarich.app` / `safepassword` → Settings → Account → cambiar email/password → Save → nuevo login

2. **Setup live tracking (iOS)**  
   Descarga *Dawarich for iOS* (App Store) → Settings → Server URL: `http://<IP>:3000` → Email + password Dawarich → Enable tracking → GPS activado

3. **Setup live tracking (Android)**  
   Descarga *Dawarich for Android* (Play Store o GitHub) → Settings → Server: `http://<IP>:3000` → Credenciales → Enable tracking

4. **Setup OwnTracks (alternativa)**  
   Instala OwnTracks → Settings → HTTP POST URL: `http://<IP>:3000/api/v1/locations` → Username + password Dawarich → Envío automático

5. **Ver mapa + historial**  
   Dashboard → Map tab (puntos tiempo real) → Timeline tab (historial por día) → Heatmap layer (intensidad)

6. **Crear viaje**  
   Trips → Create trip → Selecciona rango fechas (ej: "Jan 5 to Jan 15") → Visualiza ruta, distancia, tiempo → Añade notas, fotos

7. **Importar Google Takeout**  
   Settings → Import → Google Takeout → Sube archivo JSON (descargado de Takeout) → Procesa automático

8. **Setup family sharing**  
   Settings → Family → Invite family member → Email → Familiar acepta invite → Ver ubicaciones mutuas (consentimiento)

9. **Integrar Immich/Photoprism (opcional)**  
   Settings → Integrations → Immich o Photoprism → URL + credenciales → Importa fotos geotagged auto → Visualiza en mapa + trips

## 💡 Casos de uso

- 🔐 **Reemplazo Google Timeline** — Privacy-first, autohospedado, control total de datos
- 👨‍👩‍👧‍👦 **Family location sharing** — Saber dónde están familiares, consentimiento mutuo
- ✈️ **Travel tracking** — Crea viajes, analiza estadísticas, visualiza rutas
- 📋 **Tax residency** — Track días en cada país, reportes para impuestos
- 📸 **Photo geotagging** — Auto-organiza fotos por ubicación (Immich/Photoprism sync)
- 🛡️ **Privacy enthusiasts** — Sin Google, sin cloud lock-in, sin publicidad, solo tus datos
- 🧠 **Personal memory** — Revisa dónde estabas hace meses/años, recuerdos geolocalizados

## 🔒 Acceso remoto seguro

Para exponer Dawarich de forma segura en Internet:

1. **Reverse proxy con HTTPS** (nginx/Traefik/Caddy) + certificado Let's Encrypt
2. **Autenticación adicional** — Basic auth en proxy, o Cloudflare Access / Tailscale / WireGuard
3. **Firewall** — Restringe puerto 3000 solo a IP del proxy
4. **Headers de seguridad** — HSTS, CSP, X-Frame-Options en configuración proxy
5. **Rate limiting** — Protege endpoints de tracking (`/api/v1/locations`)

> ⚠️ **NUNCA** expongas puerto 3000 directamente a Internet sin HTTPS y autenticación.

## 🛠️ Gestión y mantenimiento

```bash
# Ver estado contenedores
docker compose -f docker/docker-compose.yml ps

# Ver logs en tiempo real
docker compose -f docker/docker-compose.yml logs -f dawarich

# Detener Dawarich
docker compose -f docker/docker-compose.yml down

# Actualizar versión
docker compose -f docker/docker-compose.yml pull
docker compose -f docker/docker-compose.yml up -d

# Backup PostgreSQL
docker compose -f docker/docker-compose.yml exec postgres pg_dump -U dawarich dawarich > backup.sql

# Restore PostgreSQL
docker compose -f docker/docker-compose.yml exec -T postgres psql -U dawarich dawarich < backup.sql

# Ejecutar migraciones tras actualización
docker compose -f docker/docker-compose.yml exec dawarich bundle exec rake db:migrate

# Monitorear consumo recursos
docker stats
# Típicamente:
# dawarich: 300-600MB RAM
# postgres: 200-400MB RAM
```

### Stack técnico
- **Backend**: Ruby on Rails 7+
- **Frontend**: Rails views + Stimulus JS + Leaflet maps
- **Database**: PostgreSQL 13+
- **Cache/Jobs**: Redis + Sidekiq
- **Maps**: Leaflet + OpenStreetMap
- **APIs**: REST API (location tracking, imports)
- **Deployment**: Docker Compose, Kubernetes (Helm)

## 📝 Licencia

**AGPL-3.0** — Código abierto, uso comercial permitido, modificaciones deben compartirse bajo misma licencia.  
Ver [LICENSE](https://github.com/Freika/dawarich/blob/main/LICENSE) en repositorio oficial.

---

> 📖 **Guía completa en el blog**: [Cómo instalar Dawarich en Docker: Location history tracker autohospedado Google Timeline](https://genbyte.blogspot.com/2026/09/como-instalar-dawarich-en-docker.html)  
> 🐙 **Repo oficial**: [Freika/dawarich](https://github.com/Freika/dawarich) | 🐳 **Docker Hub**: [freika/dawarich](https://hub.docker.com/r/freika/dawarich)