# HELIUM — Playbook de Restauración

> Si HELIUM falla, sigue este documento de arriba a abajo. Cada paso es exactamente lo que se hizo para construirlo.

**Nodo:** HELIUM — HP EliteBook 2560p  
**OS objetivo:** Debian 13 "Trixie" minimal server  
**Última actualización:** 2026-06-27  

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

- [ ] Outline (wiki)
- [ ] Authentik (SSO)

---

## Notas

- IP local actual: `192.168.1.12` (DHCP — asignar reserva estática en el router)
- Si la IP cambia, escanear la red: `nmap -sn 192.168.1.0/24` desde NEON
- Las credenciales están en `C:\NE - OS\credenciales.md` — nunca en git
