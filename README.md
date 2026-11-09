# Laboratorio MSP – Servicios IT gestionados con software libre

Laboratorio para montar, probar y documentar un **servicio de soporte IT gestionado (MSP)** orientado a pequeñas empresas,
usando únicamente herramientas *open source* desplegadas con Docker.

El objetivo es validar en local todo el stack antes de llevarlo a producción en un VPS.

## Servicios incluidos

| Servicio | Función | Puerto (lab) |
|---|---|---|
| [Uptime Kuma](https://github.com/louislam/uptime-kuma) | Monitorización de webs, servidores y puertos con alertas | `3001` |
| [UrBackup](https://www.urbackup.org/) | Copias de seguridad de archivos e imagen de equipos Windows/Linux | `55414` |
| [MeshCentral](https://meshcentral.com/) | Acceso y gestión remota (escritorio, terminal, archivos) | `443` |
| [Vaultwarden](https://github.com/dani-garcia/vaultwarden) + [Caddy](https://caddyserver.com/) | Gestor de contraseñas compatible con Bitwarden, con HTTPS | `8443` |

## Arquitectura

```
                Portátil (Windows + VirtualBox)
 ┌──────────────────────────────────────────────────────────┐
 │                Red NAT "LabMSP" 10.10.10.0/24             │
 │                                                          │
 │  ┌──────────────────────────┐   ┌─────────────────────┐  │
 │  │ MSP-Server               │   │ Client-W10          │  │
 │  │ Ubuntu Server 24.04      │   │ Windows 10/11       │  │
 │  │ 10.10.10.10 (estática)   │◄──┤ DHCP                │  │
 │  │ Docker:                  │   │ Agentes:            │  │
 │  │  - uptime-kuma           │   │  - UrBackup client  │  │
 │  │  - urbackup              │   │  - MeshAgent        │  │
 │  │  - meshcentral           │   │                     │  │
 │  │  - vaultwarden + caddy   │   │                     │  │
 │  └──────────────────────────┘   └─────────────────────┘  │
 │        ▲ 127.0.0.1:2222 → 10.10.10.10:22 (SSH)           │
 └──────────────────────────────────────────────────────────┘
```

## Requisitos

- Equipo con virtualización activada (VT-x / AMD-V), **16 GB de RAM** recomendados y ~200 GB libres.
- [VirtualBox](https://www.virtualbox.org/) 7.x
- ISO de **Ubuntu Server 24.04 LTS**
- ISO de **Windows 10/11** (sirve la versión de evaluación de Windows 11 Enterprise)

## Guía paso a paso

1. [Preparar VirtualBox y las máquinas virtuales](docs/01-virtualbox.md)
2. [Instalar Ubuntu Server y configurar la red](docs/02-ubuntu-red.md)
3. [Acceso por SSH desde el anfitrión](docs/03-ssh.md)
4. [Instalar Docker](docs/04-docker.md)
5. [Uptime Kuma – monitorización](docs/05-uptime-kuma.md)
6. [UrBackup – copias de seguridad](docs/06-urbackup.md)
7. [MeshCentral – acceso remoto](docs/07-meshcentral.md)
8. [Vaultwarden – gestor de contraseñas](docs/08-vaultwarden.md)
9. [Operación diaria del laboratorio](docs/09-operacion.md)
10. [Problemas encontrados y soluciones](docs/10-troubleshooting.md)
11. [Paso a producción (VPS + dominio)](docs/11-produccion.md)

## Estructura del repositorio

```
.
├── README.md
├── docs/                 # Guías paso a paso
├── netplan/              # Configuración de IP estática del servidor
└── servicios/            # Un compose.yaml por servicio
    ├── uptime-kuma/
    ├── urbackup/
    ├── meshcentral/
    └── vaultwarden/
```

## Despliegue rápido

Con Docker ya instalado en el servidor:

```bash
git clone https://github.com/<tu-usuario>/lab-msp.git
cd lab-msp/servicios

for s in uptime-kuma urbackup meshcentral vaultwarden; do
  (cd "$s" && docker compose up -d)
done
docker ps
```

> ⚠️ Los datos de cada servicio (`data/`, `database/`, `backups/`, `vw-data/`…) se crean junto a cada
> `compose.yaml` y están excluidos del repositorio en `.gitignore`. **Nunca subas datos, contraseñas ni certificados.**

## Estado

- [x] Laboratorio virtual y red
- [x] Docker
- [x] Uptime Kuma
- [x] UrBackup (copia y restauración probadas)
- [x] MeshCentral
- [ ] Vaultwarden
- [ ] Paso a producción

## Licencia

Documentación bajo [MIT](LICENSE). Cada herramienta mantiene su propia licencia.
