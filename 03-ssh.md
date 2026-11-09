# 3. Acceso por SSH desde el anfitrión

La consola de VirtualBox no permite copiar y pegar con Ubuntu Server (no tiene entorno gráfico).
Se trabaja por SSH desde Windows.

Como la red NAT no permite llegar a las VM desde el anfitrión, se crea un **reenvío de puertos**:

Network Manager → NAT Networks → `LabMSP` → **Reenvío de puertos** → `+`

| Nombre | Protocolo | IP anfitrión | Puerto anfitrión | IP invitado | Puerto invitado |
|---|---|---|---|---|---|
| ssh | TCP | 127.0.0.1 | 2222 | 10.10.10.10 | 22 |

Equivalente por línea de comandos:

```powershell
VBoxManage natnetwork modify --netname LabMSP --port-forward-4 "ssh:tcp:[127.0.0.1]:2222:[10.10.10.10]:22"
```

Conexión desde PowerShell / Windows Terminal:

```powershell
ssh -p 2222 <usuario>@127.0.0.1
```

Pegar en la terminal: clic derecho o `Ctrl+V` (`Ctrl+Shift+V` en algunas versiones).

## Reenvíos opcionales

Para abrir las webs desde el navegador del anfitrión:

| Nombre | Anfitrión | Invitado |
|---|---|---|
| kuma | 127.0.0.1:3001 | 10.10.10.10:3001 |
| mesh | 127.0.0.1:8443 | 10.10.10.10:443 |

## Recomendado (pendiente)

- Autenticación por clave: `ssh-keygen` en Windows y `ssh-copy-id` / copiar la clave a `~/.ssh/authorized_keys`.
- Desactivar el acceso por contraseña en `/etc/ssh/sshd_config` (`PasswordAuthentication no`).
