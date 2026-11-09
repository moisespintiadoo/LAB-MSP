# 5. Uptime Kuma – Monitorización

Vigila webs, servidores y puertos de los clientes y avisa (correo, Telegram, etc.) cuando algo cae.

## Despliegue

```bash
mkdir -p ~/servicios/uptime-kuma && cd ~/servicios/uptime-kuma
# copiar servicios/uptime-kuma/compose.yaml de este repositorio
docker compose up -d
docker ps
```

Fichero: [`servicios/uptime-kuma/compose.yaml`](../servicios/uptime-kuma/compose.yaml)

## Configuración inicial

1. Desde `Client-W10`: `http://10.10.10.10:3001`
2. Crear el usuario administrador.
3. **Añadir monitor** → HTTP(s) → `https://google.com` → intervalo 60 s.

## Monitores del propio laboratorio

| Nombre | Tipo | Destino |
|---|---|---|
| UrBackup | TCP Port | `10.10.10.10:55414` |
| MeshCentral | HTTP(s) (ignorar errores TLS) | `https://10.10.10.10` |
| Vaultwarden | HTTP(s) (ignorar errores TLS) | `https://10.10.10.10:8443` |

## Pendiente

- Configurar notificaciones (Telegram o correo).
- Página de estado pública por cliente.
