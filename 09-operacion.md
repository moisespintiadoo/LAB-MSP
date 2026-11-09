# 9. Operación diaria del laboratorio

## Arrancar

1. Encender `MSP-Server` y después `Client-W10`.
2. Conectarse y comprobar los servicios:

   ```bash
   ssh -p 2222 <usuario>@127.0.0.1
   docker ps
   ```

   Deben aparecer en `Up`: `uptime-kuma`, `urbackup`, `meshcentral`, `vaultwarden`, `vaultwarden-caddy`.

## Apagar

- Comprobar en UrBackup → **Activities** que no hay copias en curso.
- Servidor: `sudo shutdown now` o **Guardar el estado de la máquina** en VirtualBox.
- Windows: Inicio → Apagar.
- ⚠️ **Nunca** usar *Apagar la máquina* (Power off): equivale a desenchufar y puede corromper bases de datos.

## Actualizar un servicio

```bash
cd ~/servicios/<servicio>
docker compose pull
docker compose up -d
docker image prune -f
```

## Instantáneas recomendadas

| Instantánea | Momento |
|---|---|
| `Docker + Kuma OK` | Tras instalar Docker y Uptime Kuma |
| `UrBackup OK` | Tras probar copia y restauración |
| `MeshCentral OK` | Tras instalar el agente |
| `Vaultwarden OK` | Laboratorio completo |
