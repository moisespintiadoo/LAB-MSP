# 6. UrBackup – Copias de seguridad

Copias de archivos e imágenes de disco de los equipos de los clientes hacia el servidor.

## Despliegue

```bash
mkdir -p ~/servicios/urbackup && cd ~/servicios/urbackup
# copiar servicios/urbackup/compose.yaml de este repositorio
docker compose up -d
```

Fichero: [`servicios/urbackup/compose.yaml`](../servicios/urbackup/compose.yaml)

Usa `network_mode: host` para que los clientes de la red local descubran el servidor automáticamente (UDP 35623).

Puertos: `55413` (FastCGI), `55414` (web), `55415` (clientes por Internet), `35623/udp` (descubrimiento).

## Configuración del servidor

1. `http://10.10.10.10:55414`
2. **Settings → General → Server → Backup storage path:** `/backups`
3. **Settings → Users:** crear un administrador (sin él, la web queda abierta).

## Cliente Windows

1. Descargar el cliente de Windows desde urbackup.org (Download → Client).
2. Instalar con opciones por defecto; aparece solo en el servidor en 1–2 minutos.
3. Icono de la bandeja → **Añadir/eliminar carpetas de copia** → p. ej. `C:\Users`.

## Lanzar una copia

**Status** → marcar el cliente (debe estar *Online*) → elegir el tipo → **Start**:

| Tipo | Uso |
|---|---|
| Full file backup | Copia completa de las carpetas elegidas (la primera vez) |
| Incremental file backup | Solo los cambios |
| Full image backup | Imagen completa del disco |
| Incremental image backup | Cambios de la imagen |

El progreso se ve en **Activities** y los errores en **Logs**.

## Restaurar (prueba obligatoria) ✅

1. Borrar un archivo de prueba en el cliente.
2. **Backups** → cliente → copia → navegar hasta el archivo.
3. **Restore** (vuelve a su ubicación original; el cliente pide confirmación) o **Download**.

> Una copia que no se ha restaurado nunca no es una copia. Este ciclo es lo que se vende al cliente.
