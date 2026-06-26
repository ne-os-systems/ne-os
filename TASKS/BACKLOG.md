# BACKLOG — Plan de Acción NE-OS

*Aquí cae todo. Ideas, tareas técnicas, pendientes, lluvia de ideas.*  
*Última actualización: 2026-06-19*

---

## [NEON] — Core Workstation

- [P1] Renombrar equipo en Windows de `MIGLEYXIS` a `NEON`
- [P1] Cambiar nombre completo del usuario (actualmente `MIGELYXIS ANGELIMAR ALFONZO PINEDA`)
- [P2] Asignar IP estática local vía DHCP reservation en el router
- [P2] Registrar dirección MAC en REGISTRY/NETWORK.md
- [P3] Evaluar adaptador USB-C a Ethernet para mayor estabilidad en operaciones críticas

---

## [XENON] — AI Compute Node / Gaming

- [P1] Decidir distro Linux para el lado de trabajo (candidato: Pop!_OS 22.04)
- [P1] Instalar dual boot: Linux (AI / trabajo) + Windows (gaming)
- [P1] Configurar Ollama en Linux con drivers NVIDIA nativos
- [P2] Verificar y documentar modelos LLM probados y sus parámetros
- [P2] Evaluar si se expone el endpoint de Ollama a la red NE-OS (acceso desde NEON)
- [P2] Asignar IP estática local
- [P2] Registrar dirección MAC en REGISTRY/NETWORK.md
- [P3] Upgrade RAM a dual channel (agregar segundo stick del mismo modelo)
- [P3] Evaluar si XENON puede actuar como nodo de render/cómputo distribuido

---

## [HELIUM] — Servidor Activo (Windows Server 2022)

- [P1] Definir rol definitivo de servidor
  - Candidatos: DNS interno · WireGuard VPN · Monitoreo · Servidor de archivos SMB
- [P1] Configurar acceso RDP desde NEON
- [P2] Asignar IP estática fija en el router
- [P2] Registrar dirección MAC en REGISTRY/NETWORK.md
- [P2] Documentar configuración actual de Windows Server 2022 (roles instalados, estado)
- [P3] Evaluar migración a Linux si Windows Server resulta excesivo para el rol elegido

---

## [MOB-01] — Acer Nitro 5 / Estación Móvil

- [P2] Definir rol definitivo en la red
- [P2] Asignar IP estática local
- [P2] Registrar dirección MAC en REGISTRY/NETWORK.md
- [P3] Evaluar uso de RTX 3050 Mobile para inferencia AI local (secundario a XENON)
- [P3] Evaluar instalación de Linux o dual boot

---

## [RED] — Infraestructura de Red General

- [P1] Documentar modelo y capacidades del router ISP actual
- [P2] Registrar MACs de todos los dispositivos en el router
- [P2] Configurar DHCP reservations para IPs estáticas de todos los nodos
- [P3] Evaluar adquisición de switch administrado
- [P3] Diseñar segmentación VLAN
  - `VLAN-PROD` → NEON + HELIUM
  - `VLAN-COMPUTE` → XENON
  - `VLAN-MOBILE` → MOB-01

---

## [GRID] — Sistema NE-OS

- [P1] Mover `NE-OS-Naming-System.md` a `DOCS\`
- [P2] Definir estructura y propósito de `PROJECTS\`
- [P2] Definir estructura y propósito de `OPS\`
- [P2] Crear plantilla estándar para registro de nuevos dispositivos
- [P3] Crear script de escaneo automático de hardware (para onboarding de nuevos equipos)
- [P3] Evaluar sistema de versionado del REGISTRY (git en C:\NE - OS)
- [P3] Documentar flujo de trabajo completo NE-OS para nuevos equipos

---

## IDEAS / LLUVIA

*Todo lo que entra sin categoría definida. Se procesa y mueve arriba cuando madura.*

- Evaluar si HELIUM puede servir como gateway/router con doble NIC
- Considerar acceso SSH a todos los nodos (incluyendo Windows con OpenSSH)
- Dashboard web interno con estado de todos los nodos de la red
- Automatización de backups desde NEON hacia HELIUM o NAS externo
- Evaluar Tailscale o WireGuard para VPN personal entre dispositivos fuera de la red local

---

*"El backlog no es una lista de fracasos. Es un mapa de lo que viene."*
