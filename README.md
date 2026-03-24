<div align="center">

<img src="static/logo.png" alt="ACDC Logo" width="100"/>

# AC⚡DC
### Advanced Control & Deployment Console

[![Python](https://img.shields.io/badge/Python-3.8%2B-e05c2a?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-3.0%2B-e05c2a?style=flat-square&logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![Linux](https://img.shields.io/badge/Linux-Ubuntu%20%7C%20Debian-e05c2a?style=flat-square&logo=linux&logoColor=white)](https://ubuntu.com)
[![License](https://img.shields.io/badge/Licencia-MIT-f0a030?style=flat-square)](LICENSE)
[![Web](https://img.shields.io/badge/Web-unfantasmaenelsistema.com-f0a030?style=flat-square)](https://www.unfantasmaenelsistema.com)

**Consola web de administración para Linux. Gestiona tu servidor desde el navegador.**

[🚀 Instalación rápida](#instalación) · [📋 Módulos](#módulos) · [⚙️ Configuración](#configuración) · [🔒 Seguridad](#seguridad)

---

</div>

## ¿Qué es ACDC?

**ACDC** (Advanced Control & Deployment Console) es una consola web de administración de sistemas Linux, autoalojada y ligera. Similar a Webmin, pero diseñada para ser simple, rápida y sin dependencias pesadas.

Accede a tu servidor desde cualquier navegador con una interfaz moderna, modo claro/oscuro y soporte para español e inglés.

> Desarrollado por [Un Fantasma en el Sistema](https://www.unfantasmaenelsistema.com)

---

## Captura de pantalla

```
┌─────────────────────────────────────────────────────────────────┐
│  👻  AC⚡DC  Advanced Control & Deployment Console   ES  ☀  🔗 │
├──────────────┬──────────────────────────────────────────────────┤
│  Sistema     │  Dashboard                                       │
│  ◈ Dashboard │                                                  │
│  ⊡ Procesos  │  CPU 23%  ████░░░░  RAM 6.1GB  Disco 142GB      │
│  ≡ Logs      │                                                  │
│              │  Servicios activos                               │
│  Servicios   │  nginx.service    ● activo    [restart] [stop]   │
│  ▷ Systemd   │  postgresql       ● activo    [restart] [stop]   │
│  ⊕ Firewall  │  docker           ● activo    [restart] [stop]   │
│  ◷ Cron      │                                                  │
│              │                                                  │
│  Gestión     │                                                  │
│  ◎ Usuarios  │                                                  │
│  ▦ Archivos  │                                                  │
│  ⊞ Paquetes  │                                                  │
│              │                                                  │
│  Red         │                                                  │
│  ⊗ Interfaces│                                                  │
└──────────────┴──────────────────────────────────────────────────┘
```

---

## Módulos

| Módulo | Descripción |
|--------|-------------|
| ◈ **Dashboard** | CPU, RAM, disco y uptime en tiempo real (refresco cada 5s) |
| ⊡ **Procesos** | Lista de procesos con uso de CPU/MEM, filtro por nombre y señal kill |
| ≡ **Logs** | Visor de journalctl: syslog, kernel, auth/ssh, nginx, docker |
| ▷ **Systemd** | Start / stop / restart / enable de servicios del sistema |
| ⊕ **Firewall** | Estado de UFW, añadir y eliminar reglas ALLOW/DENY por puerto |
| ◷ **Cron** | Ver, añadir y eliminar tareas programadas del crontab |
| ◎ **Usuarios** | Listado de usuarios y grupos del sistema con UID/GID/shell |
| ▦ **Archivos** | Explorador web, visor de ficheros de texto y eliminación |
| ⊞ **Paquetes** | Buscar, instalar, eliminar y actualizar paquetes con APT |
| ⊗ **Red** | Interfaces de red, direcciones IP, MAC y estadísticas TX/RX |

---

## Instalación

### Requisitos previos

- Linux (Ubuntu 20.04+ / Debian 11+ recomendado)
- Python 3.8 o superior
- `systemd`, `ufw`, `apt` disponibles
- Acceso con `sudo` o como root

### Instalación con un comando

```bash
# 1. Clona el repositorio
git clone https://github.com/jpm70/ACDC.git
cd ACDC

# 2. Instala
sudo ./install.sh install
```

El script automáticamente:
- Verifica e instala Python 3 si no está presente
- Crea un entorno virtual Python aislado
- Instala las dependencias (Flask + psutil)
- Registra y arranca un servicio systemd

Una vez completado, abre el navegador en:

```
http://TU-IP:8080
```

**Credenciales por defecto:** `admin` / `admin`

> ⚠️ **Cambia la contraseña inmediatamente** tras el primer acceso:
> ```bash
> ./install.sh passwd
> ```

---

## Gestión del servicio

```bash
sudo ./install.sh start      # Arrancar
sudo ./install.sh stop       # Parar
sudo ./install.sh restart    # Reiniciar
     ./install.sh status     # Ver estado del servicio
     ./install.sh logs       # Ver logs en tiempo real
     ./install.sh passwd     # Cambiar contraseña de admin
sudo ./install.sh uninstall  # Desinstalar el servicio
```

---

## Configuración

Edita `config.json` en la raíz del proyecto:

```json
{
  "username": "admin",
  "password_hash": "8c6976e5b5410...",
  "port": 8080,
  "host": "0.0.0.0",
  "session_timeout_minutes": 60,
  "site_url": "https://www.unfantasmaenelsistema.com"
}
```

| Campo | Descripción |
|-------|-------------|
| `username` | Nombre de usuario para el acceso web |
| `password_hash` | Hash SHA-256 de la contraseña (usa `./install.sh passwd`) |
| `port` | Puerto TCP en el que escucha la aplicación |
| `host` | `0.0.0.0` para todas las interfaces, `127.0.0.1` para solo local |
| `session_timeout_minutes` | Tiempo de inactividad antes de cerrar sesión |
| `site_url` | URL del enlace en la cabecera |

Tras cualquier cambio en `config.json`:

```bash
sudo ./install.sh restart
```

---

## Personalización del logo

Reemplaza `static/logo.png` con tu propio logo (recomendado: PNG cuadrado, 128×128px o superior). La interfaz lo mostrará automáticamente en la cabecera y en la pantalla de login.

---

## Seguridad

ACDC está pensado para uso en red local o con acceso controlado. Antes de exponerlo a internet, considera estas medidas:

**Mínimo recomendado:**
- [ ] Cambiar la contraseña por defecto (`./install.sh passwd`)
- [ ] Limitar el acceso por IP con UFW: `ufw allow from 192.168.1.0/24 to any port 8080`

**Para acceso remoto seguro**, pon nginx como proxy inverso con HTTPS:

```nginx
server {
    listen 443 ssl;
    server_name acdc.tudominio.com;

    ssl_certificate     /etc/letsencrypt/live/tudominio/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/tudominio/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

En ese caso, cambia `"host": "127.0.0.1"` en `config.json` para que la app solo escuche en localhost.

---

## Estructura del proyecto

```
ACDC/
├── app.py                  # Backend Flask — toda la lógica de la API
├── config.json             # Configuración de la instancia
├── requirements.txt        # Dependencias Python (flask, psutil)
├── install.sh              # Script de instalación y gestión
├── README.md               # Este archivo
├── static/
│   └── logo.png            # Logo de la aplicación
└── templates/
    ├── index.html          # Interfaz principal (SPA)
    └── login.html          # Pantalla de autenticación
```

---

## Dependencias

| Paquete | Versión | Uso |
|---------|---------|-----|
| [Flask](https://flask.palletsprojects.com) | ≥ 3.0 | Servidor web y enrutado |
| [psutil](https://github.com/giampaolo/psutil) | ≥ 5.9 | Métricas del sistema (CPU, RAM, red, procesos) |

Sin base de datos. Sin agentes. Sin configuración compleja.

---

## Comparativa

| Característica | ACDC | Webmin | Cockpit |
|----------------|------|--------|---------|
| Instalación | 1 comando | Compleja | Media |
| Dependencias | 2 paquetes Python | Perl + módulos | Node.js |
| Peso | ~90 KB | ~30 MB | ~15 MB |
| Modo oscuro | ✅ | ❌ | Parcial |
| i18n (ES/EN) | ✅ | ✅ | ✅ |
| Logo personalizable | ✅ | ❌ | ❌ |
| Sin BD | ✅ | ❌ | ✅ |

---

## Contribuir

Las contribuciones son bienvenidas. Para cambios importantes, abre primero un issue para discutir qué te gustaría modificar.

```bash
# Fork el repo, luego:
git clone https://github.com/TU-USUARIO/ACDC.git
cd ACDC
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py   # Arranca en modo desarrollo en puerto 8080
```

---

## Licencia

MIT © [Un Fantasma en el Sistema](https://www.unfantasmaenelsistema.com)

---

<div align="center">

Hecho con ⚡ por **[Un Fantasma en el Sistema](https://www.unfantasmaenelsistema.com)**

</div>
