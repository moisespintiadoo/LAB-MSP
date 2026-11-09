# 2. Instalar Ubuntu Server y configurar la red

## Instalación

| Pantalla | Opción |
|---|---|
| Teclado | Spanish |
| Red | DHCP (la IP fija se configura después) |
| Almacenamiento | *Use an entire disk* + LVM |
| Resumen de almacenamiento | **Editar `ubuntu-lv` y darle el tamaño máximo** (por defecto solo usa ~la mitad) |
| Perfil | Servidor `msp-server`, usuario propio |
| Ubuntu Pro | Skip |
| SSH | ✅ Install OpenSSH server |
| Featured snaps | Ninguno (Docker se instala desde el repositorio oficial) |

Si se olvidó ampliar el volumen:

```bash
df -h /
sudo lvextend -r -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

## IP estática

1. Ver el nombre de la interfaz (normalmente `enp0s3`):

   ```bash
   ip a
   ```

2. Evitar que cloud-init sobrescriba la red:

   ```bash
   echo "network: {config: disabled}" | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
   ```

3. Sustituir el contenido de `/etc/netplan/50-cloud-init.yaml` por el de
   [`netplan/50-cloud-init.yaml`](../netplan/50-cloud-init.yaml) (indentación con **espacios**, nunca tabuladores):

   ```bash
   sudo nano /etc/netplan/50-cloud-init.yaml
   sudo chmod 600 /etc/netplan/50-cloud-init.yaml
   sudo netplan apply
   ```

4. Comprobar:

   ```bash
   ip a show enp0s3          # debe mostrar inet 10.10.10.10/24
   ip route                  # default via 10.10.10.1
   ping -c 3 1.1.1.1         # salida a Internet
   ping -c 3 google.com      # resolución DNS
   ```

## Quitar el repositorio del CD de instalación

Si `apt update` avisa de `file:/cdrom`:

```bash
grep -rn cdrom /etc/apt/sources.list /etc/apt/sources.list.d/
sudo sed -i 's/^deb cdrom/# deb cdrom/' /etc/apt/sources.list
```
