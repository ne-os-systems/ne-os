# NE-OS — Mapa de Red

*Estado actual de la infraestructura de red doméstica NE-OS.*  
*Última actualización: 2026-06-27*

---

## Topología actual

```
INTERNET
    │
[ROUTER ARRIS — 192.168.1.254]
    │
[Red Wi-Fi / Ethernet doméstica — 192.168.1.0/24]
    ├── NEON              192.168.1.1    Core workstation — Lenovo Ryzen 5 (Wi-Fi)
    ├── HELIUM            192.168.1.12   Servidor Docker — HP EliteBook (Debian 13) ← IP FIJA
    ├── XENON             Por asignar    AI Compute Node — Desktop i5 10th / GTX 1050 Ti
    └── NE-OS-ST-MOB-01   Por asignar    Estación móvil — Acer Nitro 5 (Wi-Fi)
```

---

## Inventario de dispositivos

| NE-OS ID | Rol | OS | IP local | Estado |
|---|---|---|---|---|
| `NEON` | Core workstation / NE-OS dev | Windows 11 Home | `192.168.1.1` | ACTIVO |
| `HELIUM` | Servidor Docker — DNS, VPN, servicios | Debian 13 "Trixie" | `192.168.1.12` ← FIJA | ACTIVO — operativo |
| `XENON` | AI Compute Node / Gaming | Windows → Dual boot pendiente | Por asignar | ACTIVO |
| `NE-OS-ST-MOB-01` | Estación móvil secundaria | Windows 11 | Por asignar | ACTIVO |

---

## Acceso remoto activo

| Servicio | Nodo destino | Dirección | Descripción |
|---|---|---|---|
| Google Remote Desktop | `NEON` | `192.168.1.1` | Acceso desde empresa |
| SSH (GRID-AI@NEON) | `HELIUM` | `192.168.1.12:22` | Administración remota desde NEON |
| AdGuard Home | `HELIUM` | `http://192.168.1.12` | Panel DNS — v0.107.77 |
| DNS NE-OS | `HELIUM` | `192.168.1.12:53` | DNS interno para toda la red |
| WireGuard VPN | `HELIUM` | `190.251.116.131:51820/UDP` | VPN accesible desde internet |
| WireGuard Panel | `HELIUM` | `http://192.168.1.12:51821` | Panel wg-easy |

---

## Roadmap de infraestructura

### Fase 1 — Identidad (en curso)
- [x] Asignar nombres NE-OS a todos los dispositivos
- [x] Registrar hardware de todos los nodos
- [ ] Renombrar equipos en Windows al ID NE-OS correspondiente
- [x] Asignar IP estática a HELIUM vía DHCP reservation — `192.168.1.12`

### Fase 2 — Activar HELIUM ✅
- [x] Instalar Debian 13 — servidor minimal sin GUI
- [x] Configurar SSH con llave GRID-AI@NEON
- [x] Instalar Docker 29.6.1
- [x] Instalar AdGuard Home — DNS operativo en `192.168.1.12:53`
- [x] Registrar MAC y reservar IP estática en router
- [x] Instalar WireGuard VPN — operativo en `190.251.116.131:51820`
- [ ] Instalar Outline (wiki)
- [ ] Instalar Authentik (SSO)

### Fase 3 — Activar XENON como nodo AI
- [ ] Instalar dual boot en XENON (Linux para AI + Windows para gaming)
- [ ] Configurar Ollama en Linux con drivers NVIDIA
- [ ] Evaluar exposición del endpoint Ollama a la red NE-OS

### Fase 4 — Red administrada
- [ ] Evaluar switch administrado
- [ ] Segmentación VLAN: `PROD` (NEON, HELIUM) / `COMPUTE` (XENON) / `MOBILE` (NE-OS-ST-MOB-01)
- [ ] Documentar modelo de router ISP y capacidades

---

## Notas de seguridad

- `XENON` tiene la mayor superficie de ataque por uso de gaming/downloads. Segmentar en VLAN separada cuando sea posible.
- `HELIUM` debe tener acceso restringido — solo desde `NEON` y servicios definidos.
- `NEON` expuesto a internet vía Google Remote Desktop — revisar configuración de seguridad periódicamente.
