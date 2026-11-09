# 7. MeshCentral – Acceso remoto

Escritorio remoto, terminal y transferencia de archivos a los equipos de los clientes, autoalojado.

## Despliegue

```bash
mkdir -p ~/servicios/meshcentral && cd ~/servicios/meshcentral
# copiar servicios/meshcentral/compose.yaml de este repositorio
docker compose up -d
docker logs -f meshcentral   # esperar "HTTPS server running on port 443", luego Ctrl+C
```

Fichero: [`servicios/meshcentral/compose.yaml`](../servicios/meshcentral/compose.yaml)

## Ajustes en `data/config.json`

Tras el primer arranque se genera `data/config.json`. Ajustes necesarios en el laboratorio:

```json
{
  "settings": {
    "cert": "10.10.10.10",
    ...
  },
  "domains": {
    "": {
      "allowedOrigin": true,
      "newAccounts": false,
      ...
    }
  }
}
```

| Ajuste | Motivo |
|---|---|
| `"cert": "10.10.10.10"` | Debe coincidir con la dirección usada en el navegador |
| `"allowedOrigin": true` | Evita el error *Invalid origin in HTTP request* (**solo laboratorio**) |
| `"newAccounts": false` | Tras crear el admin, nadie más puede registrarse |

```bash
sudo nano data/config.json
docker compose restart
docker logs --tail 20 meshcentral   # sin errores de JSON
```

## Primer acceso

1. `https://10.10.10.10` → aceptar el aviso de certificado autofirmado.
2. **Create one** → la primera cuenta es la de administrador.
3. Poner `"newAccounts": false` y reiniciar.

## Instalar el agente

1. **Add Device Group** → `Cliente Demo` → *Manage using a software agent*.
2. **Add Agent** → Windows (.exe) → ejecutar en el cliente → **Install**.
3. Si Windows bloquea el ejecutable:
   - Edge: `…` → Conservar → Mostrar más → Conservar de todos modos.
   - SmartScreen: *Más información* → *Ejecutar de todas formas*.
   - Propiedades del archivo → marcar **Desbloquear**.
   - Defender: Historial de protección → Permitir en el dispositivo.

## Pruebas

- **Desktop** → Connect
- **Terminal** → `ipconfig`
- **Files** → subir y descargar un archivo
