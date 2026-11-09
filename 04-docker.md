# 4. Instalar Docker

Instalación desde el repositorio oficial de Docker (no el snap).

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo $VERSION_CODENAME) stable" | sudo tee /etc/apt/sources.list.d/docker.list

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

Cerrar la sesión SSH y volver a entrar (para aplicar el grupo `docker`), y comprobar:

```bash
docker run hello-world
docker compose version
```

## Organización

Cada servicio vive en su propia carpeta con su `compose.yaml`:

```
~/servicios/
├── uptime-kuma/
├── urbackup/
├── meshcentral/
└── vaultwarden/
```

Todos usan `restart: unless-stopped`, por lo que arrancan solos al encender el servidor.

> 💡 Tomar una **instantánea** de la VM en VirtualBox después de cada servicio que funcione.
