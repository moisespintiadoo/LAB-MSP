# 10. Problemas encontrados y soluciones

## SSH no conecta (se queda colgado)

**Causa:** el servidor seguía con IP por DHCP (`10.10.10.4`); el reenvío apuntaba a `10.10.10.10`.
El fichero de netplan no se había guardado.

**Diagnóstico desde Windows:**

```powershell
VBoxManage dhcpserver findlease --network=LabMSP --mac-address=<MAC de la VM>
```

**Solución:** reescribir `/etc/netplan/50-cloud-init.yaml`, `sudo netplan apply`.

## `Fallo temporal al resolver «archive.ubuntu.com»`

**Causa:** sin DNS (sección `nameservers` mal indentada o no aplicada).

```bash
ping -c 3 1.1.1.1                          # ¿hay Internet?
resolvectl status enp0s3 | grep -i "dns"   # ¿hay DNS?
sudo netplan apply && sudo systemctl restart systemd-resolved
```

Después, **repetir la descarga de la clave GPG de Docker**, que también había fallado.

## `failed to connect to the docker API at unix:///var/run/docker.sock`

**Causa:** el servicio Docker no estaba arrancado.

```bash
sudo systemctl enable --now docker
```

## MeshCentral: `Invalid origin in HTTP request`

**Causa:** el nombre configurado no coincide con la dirección del navegador.

**Solución** en `data/config.json`: `"cert": "10.10.10.10"` y `"allowedOrigin": true` en `domains.""`.

## Windows bloquea el instalador del agente de MeshCentral

Ejecutable sin firmar descargado de un servidor con certificado autofirmado.
Ver opciones en [07-meshcentral.md](07-meshcentral.md#instalar-el-agente).

## `apt update` avisa de `file:/cdrom`

```bash
sudo sed -i 's/^deb cdrom/# deb cdrom/' /etc/apt/sources.list
```
