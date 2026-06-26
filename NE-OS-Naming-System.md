# NE-OS — Network Naming System

## ¿Qué es NE-OS?

NE-OS no es solo una palabra, es un juego de palabras intencional construido en dos capas:

- **NE** → del latín y griego *neos* (nuevo)
- **OS** → *Operative System*
- **NE-OS** → *New Operative System*

El guión es parte de la identidad visual de la marca: separa las dos capas de significado pero las une en una sola palabra. Nunca se escribe `NEOS`. Siempre `NE-OS`.

---

## ¿Por qué Gases Nobles?

La elección del tema **Gases Nobles** no es arbitraria: tiene raíz directa en la marca.

**NEON es un gas noble.**

Eso convierte a la tabla de gases nobles en el universo natural de NE-OS: el servidor principal lleva el nombre del elemento que da nombre a la marca, y los demás servidores son sus "hermanos" en la tabla periódica.

Los gases nobles refuerzan además la metáfora técnica:

- Son **estables** — no reaccionan fácilmente con su entorno
- Son **únicos** — cada uno tiene propiedades distintas y bien definidas
- Son **precisos** — identificados por número atómico exacto, sin ambigüedad
- Son **nobles** — la jerarquía está en el nombre

---

## Servidores — Gases Nobles

Los servidores se nombran con el nombre completo del gas noble en mayúsculas.  
El orden por peso atómico establece una jerarquía natural de ligero a denso.

| Nombre | Símbolo | N° Atómico | Rol sugerido en la red | Razón de la elección |
|--------|---------|------------|------------------------|----------------------|
| `NEON` | Ne | 10 | Servidor principal / core | El nombre de la marca. El centro de todo. |
| `HELIUM` | He | 2 | Servidor ligero / cloud / proxy | El más liviano de los gases nobles |
| `ARGON` | Ar | 18 | Servidor secundario / aplicaciones | El más abundante, el más confiable |
| `KRYPTON` | Kr | 36 | Seguridad / backup / cifrado | Denso, protegido, resistente |
| `XENON` | Xe | 54 | Alto rendimiento / HPC / render | El más potente del grupo |
| `RADON` | Rn | 86 | Monitoreo / logs / alertas | El más activo del grupo |

> **Nota:** Los roles son sugeridos, no fijos. Lo que no cambia es el nombre asignado a cada máquina — la función puede evolucionar, el nombre permanece.

---

## Estaciones de trabajo — NE-OS-ST

Prefijo: `NE-OS-ST` (*NE-OS Station*)

**Numeración simple:**
```
NE-OS-ST-01
NE-OS-ST-02
NE-OS-ST-03
```

**Con rol específico:**
```
NE-OS-ST-DEV-01      → Desarrollo
NE-OS-ST-DESIGN-01   → Diseño / Creativo
NE-OS-ST-ADMIN-01    → Administración
NE-OS-ST-OPS-01      → Operaciones
```

La numeración siempre con dos dígitos (`01`, `02`...) para mantener orden alfabético consistente.

---

## Dispositivos de red — NE-OS prefijado

Todos los dispositivos de red llevan el prefijo `NE-OS-` seguido del tipo y número.

| Dispositivo | Nombre | Descripción |
|-------------|--------|-------------|
| Gateway / Router | `NE-OS-GW` | Puerta de entrada a la red |
| Switch principal | `NE-OS-SW-01` | Distribución de red |
| Access Point | `NE-OS-AP-01` | Punto de acceso WiFi |
| Firewall | `NE-OS-FW` | Perímetro de seguridad |
| NAS / Storage | `NE-OS-NAS-01` | Almacenamiento en red |
| VPN | `NE-OS-VPN` | Túnel seguro externo |

---

## Resumen de la convención

```
Servidores   →  [GAS NOBLE]                →  NEON / HELIUM / ARGON / KRYPTON / XENON / RADON
Estaciones   →  NE-OS-ST-[ROL]-[NN]       →  NE-OS-ST-DEV-01
Red          →  NE-OS-[TIPO]-[NN]          →  NE-OS-GW / NE-OS-SW-01 / NE-OS-AP-01
```

---

## Cuentas de usuario — Convención

Las cuentas de usuario siguen el patrón `[nombre] · NE-OS`.  
El punto medio `·` separa la identidad personal de la marca, igual que el guión en NE-OS.

| Campo | Formato | Ejemplo |
|---|---|---|
| Nombre de usuario (login) | Nombre en MAYÚSCULAS | `NEON` |
| Nombre completo (display) | `NE-OS` | `NE-OS` |

**Nota:** El nombre de usuario (login) determina la ruta del perfil (`C:\Users\[login]`).  
No se cambia sin un formateo limpio — es una deuda técnica aceptada y documentada hasta el próximo ciclo de reinstalación.

---

## Reglas de escritura de marca

La marca **NE-OS** tiene una sola forma correcta:

| Contexto | Correcto | Incorrecto |
|----------|----------|------------|
| Nombre de marca | `NE-OS` | `NEOS`, `Neos`, `neos` |
| Prefijo de equipo | `NE-OS-ST-01` | `NEOS-ST-01`, `Ne-Os-ST-01` |
| Código / scripts | `ne-os` | `neos`, `NeOs` |

El guión es parte de la identidad. No es opcional.

---

*NE-OS Naming System — v1.0*
