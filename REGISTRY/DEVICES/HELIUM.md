# HELIUM — Servidor Legacy Activo

> *"Viejo en silicio. El programa más antiguo del grid sigue corriendo — y ahora tiene un propósito claro."*

---

## Identidad

| Campo | Valor |
|---|---|
| NE-OS ID | `HELIUM` |
| Tipo | Laptop Legacy / Servidor Activo |
| Estado | ACTIVO — En espera de rol definitivo (servidor operativo) |
| Fecha de registro | 2026-06-19 |

---

## Hardware

| Componente | Detalle |
|---|---|
| Fabricante / Modelo | HP EliteBook 2560p |
| CPU | Intel Core i5-2540M @ 2.60 GHz |
| RAM | 6 GB |
| Almacenamiento | SATA SSD — 220 GB |
| OS instalado | **Windows Server 2022** — instalado y funcional |

---

## Rol en la red

**Servidor legacy activo. Sistema operativo de servidor ya instalado y limpio.**

### Estado actual

Windows Server 2022 instalado, funcional y limpio. Proyecto suspendido temporalmente.  
**No es un candidato a servidor — ya es un servidor.** Solo necesita un rol definido.

### Por qué HELIUM

HELIUM es el gas noble más ligero y el más antiguo en origen cósmico.  
Este equipo es el más antiguo del grid. Su hardware es limitado, pero Windows Server 2022 lo convierte en un servidor real desde el primer día.  
La ligereza de HELIUM describe exactamente lo que debe hacer: tareas de servidor que no requieren músculo, sino constancia.

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

## Pendiente

- [ ] Definir rol de servidor definitivo
- [ ] Asignar IP estática fija en el router
- [ ] Configurar acceso remoto (RDP / WinRM) desde NEON
- [ ] Documentar configuración actual de Windows Server 2022

---

## Historial de cambios

| Fecha | Cambio |
|---|---|
| 2026-06-19 | Registro inicial. Confirmado Windows Server 2022 instalado y funcional. Estado actualizado de "candidato" a "servidor activo en espera de rol". |
