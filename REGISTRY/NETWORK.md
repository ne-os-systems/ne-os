# NE-OS — Mapa de Red

*Estado actual de la infraestructura de red doméstica NE-OS.*  
*Última actualización: 2026-06-19*

---

## Topología actual

```
INTERNET
    │
[ROUTER / ISP]
    │
[Red Wi-Fi / Ethernet doméstica]
    ├── NEON              → Core workstation — Lenovo Ryzen 5 (Wi-Fi)
    ├── XENON             → AI Compute Node — Desktop i5 10th / GTX 1050 Ti
    ├── HELIUM            → Servidor activo — HP EliteBook (Windows Server 2022)
    └── NE-OS-ST-MOB-01   → Estación móvil — Acer Nitro 5 (Wi-Fi)
```

---

## Inventario de dispositivos

| NE-OS ID | Rol | OS | IP local | Estado |
|---|---|---|---|---|
| `NEON` | Core workstation / NE-OS dev | Windows 11 Home | Por asignar | ACTIVO |
| `XENON` | AI Compute Node / Gaming | Windows 10 → Dual boot (pendiente) | Por asignar | ACTIVO |
| `HELIUM` | Servidor legacy | Windows Server 2022 | Por asignar | ACTIVO — sin rol |
| `NE-OS-ST-MOB-01` | Estación móvil secundaria | Windows 11 | Por asignar | ACTIVO |

---

## Acceso remoto activo

| Servicio | Nodo destino | Dirección | Descripción |
|---|---|---|---|
| Google Remote Desktop | `NEON` | — | Acceso desde empresa |

---

## Roadmap de infraestructura

### Fase 1 — Identidad (en curso)
- [x] Asignar nombres NE-OS a todos los dispositivos
- [x] Registrar hardware de todos los nodos
- [ ] Renombrar equipos en Windows al ID NE-OS correspondiente
- [ ] Asignar IPs estáticas vía DHCP reservation en el router

### Fase 2 — Activar HELIUM
- [ ] Definir rol definitivo para HELIUM (DNS / VPN / Monitoreo)
- [ ] Configurar acceso RDP a HELIUM desde NEON
- [ ] Registrar MAC de HELIUM y reservar IP estática

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
