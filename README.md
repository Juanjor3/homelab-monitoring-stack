# Homelab Monitoring & Observability Stack

Stack modular en **Docker Compose** para centralizar la observabilidad de un servidor Linux con **Homepage**, **Uptime Kuma** y **Dozzle**, con integración opcional para [docker-cve-radar](https://github.com/Juanjor3/docker-cve-radar).

> **Nota sobre la configuración:**  
> La arquitectura de monitorización, las comprobaciones de salud y los criterios operativos han sido definidos por el autor con apoyo de **herramientas de IA**, dejando las plantillas limpias y parametrizadas para uso genérico.

---

## Servicios Incluidos

| Servicio | Puerto por defecto | Función |
| :--- | :--- | :--- |
| **Homepage** (`ghcr.io/gethomepage/homepage`) | `3000` | Panel unificado de acceso a servicios y métricas Docker |
| **Uptime Kuma** (`louislam/uptime-kuma:1`) | `3001` | Monitorización continua HTTP/TCP/Docker y alertas |
| **Dozzle** (`amir20/dozzle`) | `8888` | Visor ligero de logs de contenedores en tiempo real |

Además, `homepage/config/services.yaml` incluye un widget `customapi` preconfigurado para conectar con **Docker CVE Radar** en el puerto `8090`.

---

## Buenas Prácticas Aplicadas

- **Acceso Docker en solo lectura (`:ro`):** Tanto `homepage` como `dozzle` montan `/var/run/docker.sock:ro`.
- **Rotación de logs `json-file`:** Todos los servicios limitan sus logs a `10m x 3` ficheros para evitar saturar el disco.
- **Límites de memoria:** Cada contenedor define `deploy.resources.limits.memory`.
- **Healthchecks:** Comprobaciones integradas para detectar bloqueos silenciosos.

---

## Despliegue Rápido

```bash
cp .env.example .env
docker compose up -d
```

- **Homepage:** `http://localhost:3000`
- **Uptime Kuma:** `http://localhost:3001`
- **Dozzle:** `http://localhost:8888`

Personaliza tus servicios editando `homepage/config/services.yaml` y `homepage/config/bookmarks.yaml`.
