# NE-OS-ST-MOB-01 — Estación Móvil

> *"El grid se mueve contigo."*

---

## Identidad

| Campo | Valor |
|---|---|
| NE-OS ID | `NE-OS-ST-MOB-01` |
| Tipo | Laptop / Mobile Workstation |
| Estado | ACTIVO — Secundario |
| Fecha de registro | 2026-06-19 |

---

## Hardware

| Componente | Detalle |
|---|---|
| Fabricante | Acer Nitro 5 |
| CPU | Intel Core i5 — 12ª generación |
| RAM | 8 GB |
| GPU | NVIDIA RTX 3050 Mobile |
| Almacenamiento | M.2 NVMe — 512 GB SSD |

---

## Rol en la red

**Estación móvil secundaria. Potencial nodo de cómputo GPU.**

### Funciones actuales

- Workstation secundaria / portátil
- Rol de respaldo para trabajo cuando NEON no está disponible

### Potencial no explotado

La RTX 3050 Mobile la convierte en candidata para:
- Inferencia de modelos de IA locales (LLM, Stable Diffusion, Whisper)
- Tareas de rendering o procesamiento GPU
- Nodo de cómputo secundario en la red NE-OS

### Por qué NE-OS-ST-MOB-01

Clasificada como workstation (`ST`) porque su rol principal es de estación de trabajo.  
El sufijo `-MOB` indica su naturaleza portátil y propósito móvil.  
El `01` la posiciona como la primera (y actualmente única) estación móvil del grid.

---

## Pendiente

- [ ] Definir rol definitivo en la red
- [ ] Evaluar si se configura para inferencia de IA local
- [ ] Asignar IP estática local

---

## Historial de cambios

| Fecha | Cambio |
|---|---|
| 2026-06-19 | Registro inicial. Rol provisional asignado: Mobile Workstation. |
