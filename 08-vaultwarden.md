# 8. Vaultwarden – Gestor de contraseñas

Servidor compatible con Bitwarden (apps, extensiones y clientes oficiales).

**Requiere HTTPS**: por HTTP el navegador no permite la criptografía web y la interfaz no carga.
Por eso va detrás de **Caddy** con un certificado interno, en el puerto `8443` (el `443` lo usa MeshCentral).

## Despliegue

```bash
mkdir -p ~/servicios/vaultwarden && cd ~/servicios/vaultwarden
# copiar servicios/vaultwarden/compose.yaml y Caddyfile de este repositorio
docker compose up -d
docker ps
docker logs --tail 20 vaultwarden-caddy
```

Ficheros:
- [`servicios/vaultwarden/compose.yaml`](../servicios/vaultwarden/compose.yaml)
- [`servicios/vaultwarden/Caddyfile`](../servicios/vaultwarden/Caddyfile)

Vaultwarden solo escucha en `127.0.0.1:8080`; todo el acceso pasa por Caddy.

## Primer acceso

1. `https://10.10.10.10:8443` → aceptar el aviso de certificado.
2. **Crear cuenta** con una contraseña maestra fuerte (no se puede recuperar).
3. Cerrar registros:

   ```bash
   sed -i 's/SIGNUPS_ALLOWED=true/SIGNUPS_ALLOWED=false/' compose.yaml
   docker compose up -d
   ```

## Pruebas

- Guardar una credencial de prueba.
- Crear una **Organización** (`Cliente Demo`) para compartir contraseñas con sus empleados.
