# 11. Paso a producción (VPS + dominio)

## Costes orientativos (noviembre 2026)

| Concepto | Coste aproximado |
|---|---|
| Dominio `.es` / `.com` | 7–12 €/año |
| VPS 2 vCPU / 4 GB RAM (UE) | 6–10 €/mes |
| Almacenamiento para copias (Storage Box / S3) | 3–10 €/mes |
| **Total** | **~10–20 €/mes** |

Precios cambiantes: verificar siempre en la web del proveedor. Servidor en la UE por RGPD.

## Plan

1. **Dominio** y DNS (p. ej. Cloudflare). Subdominios:
   - `status.midominio.es` → Uptime Kuma
   - `backup.midominio.es` → UrBackup
   - `remoto.midominio.es` → MeshCentral
   - `vault.midominio.es` → Vaultwarden
2. **VPS** con Ubuntu Server 24.04 LTS.
3. **Bastionado:**
   - Usuario sin root, SSH solo con clave, `PasswordAuthentication no`.
   - Firewall `ufw`: solo 22, 80, 443 y puertos de UrBackup para clientes por Internet (55415).
   - `fail2ban` y actualizaciones automáticas (`unattended-upgrades`).
4. **Proxy inverso** (Caddy) con certificados Let's Encrypt para todos los subdominios.
5. Reconfigurar MeshCentral: quitar `allowedOrigin: true`, poner el dominio real en `cert`.
6. **Almacenamiento de copias** separado del VPS.
7. **Copias del propio servidor** (configuraciones y bases de datos de los servicios).

## Estrategia de copias para clientes

- Copia local rápida en la oficina del cliente (NAS / disco), facturada al cliente.
- Copia remota de lo crítico hacia el servidor (regla 3-2-1).
