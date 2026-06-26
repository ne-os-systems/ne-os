# XENON — Nodo de Cómputo AI / Gaming

> *"El más denso de los nobles. El que más potencia entrega cuando se le pide."*

---

## Identidad

| Campo | Valor |
|---|---|
| NE-OS ID | `XENON` |
| Tipo | Desktop / AI Compute Node |
| Estado | ACTIVO — Clasificación en revisión inicial como PLAY, corregido a XENON |
| Fecha de registro | 2026-06-19 |

---

## Hardware

| Componente | Detalle |
|---|---|
| Tipo | Desktop (torre) |
| CPU | Intel Core i5 — 10ª generación |
| RAM | 16 GB (single channel — pendiente upgrade a dual channel) |
| GPU | ASUS GTX 1050 Ti — 4 GB VRAM (GPU de escritorio) |
| Almacenamiento OS | SSD SATA — 240 GB |
| Almacenamiento Data | HDD — 1 TB (media / juegos) |
| OS actual | Windows 10 |

---

## Rol en la red

**Nodo de cómputo AI y estación dual propósito.**

### Por qué XENON

XENON es el gas noble de mayor rendimiento — el que se usa en láseres de alta potencia, sistemas de iluminación de alta intensidad y propulsión iónica.  
Este equipo tiene la GPU de escritorio más potente del grid. Una GTX 1050 Ti de escritorio supera en rendimiento sostenido a las GPUs móviles del resto de la red.  
**Probado con Ollama** corriendo múltiples modelos de varios miles de millones de parámetros. El grid tiene su nodo de IA.

### Funciones

- **Nodo de inferencia AI** — Ollama, modelos LLM locales, Stable Diffusion
- **Estación gaming / media** — uso de ocio en Windows
- **Nodo de cómputo distribuido** — GPU disponible para tareas de red cuando está idle

---

## Arquitectura de sistema propuesta

### Dual Boot

```
Disco SSD 240GB
├── Partición Linux    → AI / Cómputo / Trabajo (Ollama, Docker, herramientas)
└── Partición Windows  → Gaming / Media / Ocio
```

### Opciones de Linux recomendadas

| Distro | Ventaja para este caso |
|---|---|
| **Pop!_OS 22.04** | Soporte NVIDIA out-of-the-box, drivers automáticos |
| **Ubuntu 24.04 LTS** | Máxima compatibilidad, Ollama y CUDA bien documentados |
| **Fedora 40** | Más actualizado, buen soporte NVIDIA con negativo RPM Fusion |

### Modelos AI probados

- Ollama funcional con modelos multi-B de parámetros (confirmar lista exacta)

---

## Pendiente

- [ ] Decidir distro Linux para el lado de trabajo
- [ ] Instalar dual boot (Linux trabajo + Windows gaming)
- [ ] Configurar Ollama en Linux con drivers NVIDIA
- [ ] Upgrade RAM a dual channel (mismo modelo, segundo stick) — impacto significativo en rendimiento
- [ ] Asignar IP estática en la red
- [ ] Evaluar si se expone el endpoint de Ollama a la red NE-OS

---

## Historial de cambios

| Fecha | Cambio |
|---|---|
| 2026-06-19 | Registro inicial. Clasificado erróneamente como NE-OS-ST-PLAY-01. Corregido a XENON tras análisis completo de capacidades. |
