# HELIUM — Servidor Legacy Activo

> *"Viejo en silicio. El programa más antiguo del grid sigue corriendo — y ahora tiene un propósito claro."*

---

## Identidad

| Campo | Valor |
|---|---|
| NE-OS ID | `HELIUM` |
| Tipo | Laptop Legacy / Servidor Activo |
| Estado | ACTIVO — Debian 13 instalado. Configuración en progreso. |
| Fecha de registro | 2026-06-19 |

---

## Hardware

| Componente | Detalle |
|---|---|
| Fabricante / Modelo | HP EliteBook 2560p |
| CPU | Intel Core i5-2540M @ 2.60 GHz |
| RAM | 6 GB |
| Almacenamiento | SATA SSD — 220 GB |
| OS instalado | **Debian 13 "Trixie"** — instalación minimal server (sin GUI) |

---

## Rol en la red

**Servidor legacy activo. Debian 13 operativo — administrado remotamente desde NEON vía SSH.**

### Estado actual

Debian 13 "Trixie" instalado, minimal server sin GUI. SSH activo, administración 100% remota desde NEON.  
**Siguiente paso: instalar Docker y levantar los servicios del grid.**

### Por qué HELIUM

HELIUM es el gas noble más ligero y el más antiguo en origen cósmico.  
Este equipo es el más antiguo del grid. Su hardware es limitado, pero Debian 13 lo convierte en un servidor eficiente — Linux headless consume ~390MB de RAM vs los ~2.5GB de Windows Server.  
La ligereza de HELIUM describe exactamente lo que debe hacer: correr servicios 24/7 sin músculo, con constancia.

### Roles candidatos (por consumo de recursos, de menor a mayor)

| Servicio | RAM estimada | CPU | Viabilidad |
|---|---|---|---|
| DNS interno (Pi-hole / AdGuard Home) | ~200 MB | Mínimo | ✅ Excelente |
| WireGuard VPN Gateway | ~50 MB | Mínimo | ✅ Excelente |
| Servidor DHCP | ~100 MB | Mínimo | ✅ Excelente |
| Monitoreo (Prometheus + Grafana) | ~1-2 GB | Bajo | ✅ Viable |
| Servidor de archivos (SMB) | ~500 MB | Bajo | ✅ Viable |
| Web server interno (IIS / Nginx) | ~300 MB | Bajo | ✅ Viable |
| Active Directory Domain Services | ~2-3 GB | Moderado | ⚠️ Ajustado |

---

## Acceso remoto

| Campo | Valor |
|---|---|
| IP local | `192.168.1.12` |
| MAC (ethernet) | `2c:41:38:12:6f:53` — interfaz `enp0s25` |
| SSH | `ssh neos@192.168.1.12` |
| Usuario | `neos` — sudo sin contraseña |
| Llave autorizada | `GRID-AI@NEON` (ed25519) |
| Root SSH | Desactivado |
| Password SSH | Desactivado |
| Suspensión | Desactivada — corre 24/7 con pantalla cerrada |

---

## Recursos (baseline — 2026-06-27)

| Recurso | Total | Usado | Disponible |
|---|---|---|---|
| RAM | 5.7 GB | 390 MB | 5.3 GB |
| Disco | 213 GB | 1.2 GB | 201 GB |
| Temperatura máx | — | 43°C idle | — |
| Carga CPU | — | 0.01 (idle) | — |

---

## Pendiente

- [ ] Instalar Docker
- [ ] Levantar AdGuard Home (DNS)
- [ ] Levantar WireGuard (VPN)
- [ ] Levantar Outline (wiki)
- [ ] Levantar Authentik (SSO)
- [ ] Asignar IP estática fija en el router

---

## Historial de cambios

| Fecha | Cambio |
|---|---|
| 2026-06-19 | Registro inicial. Confirmado Windows Server 2022 instalado y funcional. Estado actualizado de "candidato" a "servidor activo en espera de rol". |
| 2026-06-27 | OS migrado a Debian 13 "Trixie" minimal server. Windows Server 2022 eliminado. Usuario `neos` / `NE-OS` configurado con sudo. |
| 2026-06-27 | SSH configurado con llave GRID-AI@NEON. Root SSH desactivado. Password auth desactivado. Suspensión desactivada. Administración 100% remota operativa. |
