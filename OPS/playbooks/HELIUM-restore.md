# HELIUM — Playbook de Restauración

> Si HELIUM falla, sigue este documento de arriba a abajo. Cada paso es exactamente lo que se hizo para construirlo.

**Nodo:** HELIUM — HP EliteBook 2560p  
**OS objetivo:** Debian 13 "Trixie" minimal server  
**Última actualización:** 2026-06-28  

---

## Requisitos previos

- USB con Debian 13 "Trixie" netinst amd64 (debian.org/distrib/netinst)
- Rufus configurado en MBR (no GPT) — el equipo arranca en Legacy BIOS
- Cable Ethernet conectado a HELIUM durante la instalación
- Llave pública SSH de NEON disponible

---

## Fase 1 — Instalación del OS

### 1.1 Preparar el USB (desde NEON)

1. Descargar ISO: Debian 13 "Trixie" netinst amd64
2. Abrir Rufus:
   - Esquema de partición: **MBR**
   - Sistema de destino: **BIOS o UEFI-CSM**
   - Modo: **ISO**
3. Cuando Rufus pregunte por Grub → elegir **Sí** (descargar core.img compatible)

### 1.2 Boot desde USB

1. Conectar USB a HELIUM
2. Encender → presionar `F9` para menú de arranque one-time
3. Seleccionar el USB
4. En Rufus pregunta ISO o DD → **ISO**

### 1.3 Instalador Debian

| Pantalla | Valor |
|---|---|
| Language | English |
| Country | Colombia |
| Keymap | Spanish Latin American |
| Hostname | `helium` |
| Domain | `ne-os.local` |
| Root password | Ver `credenciales.md` |
| Full name | `NE-OS` |
| Username | `neos` |
| User password | Ver `credenciales.md` |
| Partitioning | Guided — use entire disk and set up LVM |
| Disco | `/dev/sda` |
| Scheme | All files in one partition |
| Write changes | Yes |
| Software | **Solo:** SSH server + standard system utilities |
| GRUB | Instalar en `/dev/sda` |

---

## Fase 2 — Configuración post-instalación

### 2.1 Instalar sudo y agregar neos

```bash
# Conectar como root temporalmente (se desactivará después)
apt-get install -y sudo
usermod -aG sudo neos
```

### 2.2 Configurar sudo sin contraseña para neos

```bash
echo 'neos ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/neos
chmod 440 /etc/sudoers.d/neos
```

### 2.3 Instalar llave SSH de NEON

```bash
# Ejecutar como neos
mkdir -p ~/.ssh
chmod 700 ~/.ssh
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB3soMJcuo3Wy9xwHgJmuaFDqCcWudJQpoxZ5cvDeTuL GRID-AI@NEON" > ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 2.4 Endurecer SSH

Editar `/etc/ssh/sshd_config`:

```bash
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
systemctl restart sshd
```

### 2.5 Desactivar suspensión (laptop servidor)

```bash
sed -i 's/#HandleLidSwitch=suspend/HandleLidSwitch=ignore/' /etc/systemd/logind.conf
sed -i 's/#HandleLidSwitchExternalPower=suspend/HandleLidSwitchExternalPower=ignore/' /etc/systemd/logind.conf
sed -i 's/#IdleAction=ignore/IdleAction=ignore/' /etc/systemd/logind.conf
systemctl restart systemd-logind
```

### 2.6 Verificar desde NEON

```bash
ssh neos@192.168.1.12 "uptime"
```

Si responde → HELIUM restaurado y accesible. Continuar con Fase 3.

---

## Fase 3 — Docker y servicios

### 3.1 Instalar Docker

```bash
sudo apt-get install -y curl
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker neos
```

Verificar:
```bash
docker --version
sudo docker run hello-world
```

> Versión instalada: Docker 29.6.1

### 3.2 AdGuard Home (DNS)

```bash
mkdir -p /opt/ne-os/adguard/{work,conf}
cat > /opt/ne-os/adguard/docker-compose.yml << 'EOF'
services:
  adguard:
    image: adguard/adguardhome:latest
    container_name: adguard
    restart: unless-stopped
    ports:
      - '53:53/tcp'
      - '53:53/udp'
      - '3000:3000/tcp'
      - '80:80/tcp'
    volumes:
      - ./work:/opt/adguardhome/work
      - ./conf:/opt/adguardhome/conf
EOF
cd /opt/ne-os/adguard && sudo docker compose up -d
```

Completar wizard en `http://192.168.1.12:3000`:

| Campo | Valor |
|---|---|
| DNS listen interface | All interfaces |
| Web interface port | 80 |
| Admin username | `neos` |
| Admin password | Ver `credenciales.md` (mín. 8 caracteres) |

Panel final: `http://192.168.1.12`  
DNS de la red apunta a: `192.168.1.12:53`

> **Importante:** Asignar IP estática al router para que HELIUM siempre sea `192.168.1.12`

### 3.3 WireGuard VPN (wg-easy)

```bash
mkdir -p /opt/ne-os/wireguard/data

# Generar hash de contraseña
docker run --rm ghcr.io/wg-easy/wg-easy wgpw 'TU_CONTRASEÑA'

# Crear docker-compose.yml
cat > /opt/ne-os/wireguard/docker-compose.yml << 'EOF'
services:
  wg-easy:
    image: ghcr.io/wg-easy/wg-easy:latest
    container_name: wireguard
    restart: unless-stopped
    environment:
      - LANG=es
      - WG_HOST=<IP_PUBLICA>         # curl -4 ifconfig.me
      - PASSWORD_HASH=<HASH_BCRYPT>  # output del comando wgpw
      - WG_PORT=51820
      - WG_DEFAULT_DNS=192.168.1.12
      - WG_ALLOWED_IPS=0.0.0.0/0
      - UI_PORT=51821
    volumes:
      - ./data:/etc/wireguard
    ports:
      - '51820:51820/udp'
      - '51821:51821/tcp'
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.ip_forward=1
      - net.ipv4.conf.all.src_valid_mark=1
EOF

cd /opt/ne-os/wireguard && sudo docker compose up -d
```

Panel: `http://192.168.1.12:51821`  
Credenciales: ver `credenciales.md`  
Puerto a abrir en router: `51820/UDP`

### 3.4 Red Docker compartida

```bash
sudo docker network create ne-os
```

Todos los servicios que se comunican entre sí deben usar esta red (`external: true` en cada compose).

### 3.5 FreeDNS (actualizador de IP dinámica)

```bash
mkdir -p /opt/ne-os/freedns
cat > /opt/ne-os/freedns/docker-compose.yml << 'EOF'
services:
  freedns:
    image: alpine:latest
    container_name: freedns
    restart: unless-stopped
    dns:
      - 8.8.8.8
      - 8.8.4.4
    command: >
      sh -c "while true; do
        wget -qO- 'https://freedns.afraid.org/dynamic/update.php?<TOKEN_NEOS>';
        sleep 300;
      done"
EOF
cd /opt/ne-os/freedns && sudo docker compose up -d
```

Token en `credenciales.md` → sección FreeDNS.

### 3.6 Caddy (reverse proxy HTTPS)

```bash
mkdir -p /opt/ne-os/caddy/{data,config}
cat > /opt/ne-os/caddy/Caddyfile << 'EOF'
ne-os.mooo.com {
    reverse_proxy outline:3000
}

neosvault.mooo.com {
    reverse_proxy vaultwarden:80
}
EOF

cat > /opt/ne-os/caddy/docker-compose.yml << 'EOF'
services:
  caddy:
    image: caddy:latest
    container_name: caddy
    restart: unless-stopped
    dns:
      - 8.8.8.8
      - 8.8.4.4
    ports:
      - '80:80'
      - '443:443'
      - '443:443/udp'
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
    networks:
      - ne-os

networks:
  ne-os:
    external: true
EOF
cd /opt/ne-os/caddy && sudo docker compose up -d
```

Caddy obtiene certificados Let's Encrypt automáticamente. Requiere que los dominios ya resuelvan a la IP pública.

### 3.7 AdGuard (versión corregida — puerto 8080)

> El puerto 80 lo usa Caddy. AdGuard va en 8080.

```bash
cat > /opt/ne-os/adguard/docker-compose.yml << 'EOF'
services:
  adguard:
    image: adguard/adguardhome:latest
    container_name: adguard
    restart: unless-stopped
    ports:
      - '53:53/tcp'
      - '53:53/udp'
      - '3000:3000/tcp'
      - '8080:80/tcp'
    volumes:
      - ./work:/opt/adguardhome/work
      - ./conf:/opt/adguardhome/conf
EOF
cd /opt/ne-os/adguard && sudo docker compose up -d
```

Panel: `http://192.168.1.12:8080`

### 3.8 Outline (wiki)

```bash
mkdir -p /opt/ne-os/outline
cat > /opt/ne-os/outline/docker-compose.yml << 'EOF'
services:
  outline:
    image: outlinewiki/outline:latest
    container_name: outline
    restart: unless-stopped
    depends_on:
      - postgres
      - redis
      - minio
    environment:
      - SECRET_KEY=<ver credenciales.md>
      - UTILS_SECRET=<ver credenciales.md>
      - DATABASE_URL=postgres://outline:outline@postgres:5432/outline?sslmode=disable
      - REDIS_URL=redis://redis:6379
      - URL=https://ne-os.mooo.com
      - FORCE_HTTPS=true
      - GITHUB_CLIENT_ID=<ver credenciales.md>
      - GITHUB_CLIENT_SECRET=<ver credenciales.md>
      - AWS_ACCESS_KEY_ID=minio
      - AWS_SECRET_ACCESS_KEY=<ver credenciales.md>
      - AWS_REGION=us-east-1
      - AWS_S3_UPLOAD_BUCKET_URL=http://outline-minio:9000
      - AWS_S3_UPLOAD_BUCKET_NAME=outline
      - AWS_S3_FORCE_PATH_STYLE=true
      - FILE_STORAGE=s3
    networks:
      - ne-os
  postgres:
    image: postgres:16
    container_name: outline-postgres
    restart: unless-stopped
    environment:
      - POSTGRES_USER=outline
      - POSTGRES_PASSWORD=outline
      - POSTGRES_DB=outline
    volumes:
      - ./postgres:/var/lib/postgresql/data
    networks:
      - ne-os
  redis:
    image: redis:7
    container_name: outline-redis
    restart: unless-stopped
    networks:
      - ne-os
  minio:
    image: minio/minio
    container_name: outline-minio
    restart: unless-stopped
    environment:
      - MINIO_ROOT_USER=minio
      - MINIO_ROOT_PASSWORD=<ver credenciales.md>
    volumes:
      - ./minio:/data
    command: server /data --console-address ":9001"
    networks:
      - ne-os

networks:
  ne-os:
    external: true
EOF
cd /opt/ne-os/outline && sudo docker compose up -d
```

URL: `https://ne-os.mooo.com` — login con GitHub (`ne-os-systems`)

### 3.9 Vaultwarden (gestor de contraseñas)

```bash
mkdir -p /opt/ne-os/vaultwarden
cat > /opt/ne-os/vaultwarden/docker-compose.yml << 'EOF'
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      - DOMAIN=https://neosvault.mooo.com
      - SIGNUPS_ALLOWED=true
    volumes:
      - ./data:/data
    networks:
      - ne-os

networks:
  ne-os:
    external: true
EOF
cd /opt/ne-os/vaultwarden && sudo docker compose up -d
```

URL: `https://neosvault.mooo.com`
> Después de crear la primera cuenta, cambiar `SIGNUPS_ALLOWED=true` a `false` y reiniciar.

---

## Configuración del router ARRIS (Tigo)

Panel: `192.168.1.254` — credenciales en `credenciales.md`

### DHCP Reservation
| Nombre | IP | MAC |
|---|---|---|
| helium | 192.168.1.12 | 2C:41:38:12:6F:53 |

### Port Forwarding (Virtual Servers)
| Nombre | Puerto externo | Protocolo | IP destino | Puerto interno |
|---|---|---|---|---|
| WireGuard | 51820 | UDP | 192.168.1.12 | 51820 |
| HELIUMHTTP | 80 | TCP | 192.168.1.12 | 80 |
| HELIUMHTTPS | 443 | TCP | 192.168.1.12 | 443 |

> Puerto 80 y 443: necesarios para HTTPS (Caddy + Let's Encrypt)
> Puerto 51820 UDP: VPN WireGuard

---

## Notas

- Si la IP pública cambia, WireGuard seguirá funcionando porque FreeDNS actualiza el dominio automáticamente. Actualizar `WG_HOST` en wireguard compose si se migra a dominio.
- Las credenciales están en `C:\NE - OS\credenciales.md` — nunca en git
- Backup de datos de servicios: `/opt/ne-os/*/` en HELIUM
