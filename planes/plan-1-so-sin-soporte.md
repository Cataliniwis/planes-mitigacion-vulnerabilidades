# Plan 1 · Sistema operativo Ubuntu 16.04 — Fin de vida (SEoL)

**Severidad:** Crítica — CVSS 10.0 · **Fuente:** Nessus Essentials (Plugin 201351)

| Campo | Detalle |
|---|---|
| CVE | Sin CVE específico (condición de fin de vida) |
| CWE | CWE-1104: Use of Unmaintained Third Party Components |
| Host / puerto | `<VICTIMA>` — 22/tcp (detección vía banner SSH) |
| Marco normativo | ISO/IEC 27002:2022 Control 8.8 · CIS v8.1 Control 2 · NIST CSF 2.0 ID.AM-2 / PR.IP-12 |

**Contexto:** Ubuntu 16.04 LTS llegó a fin de soporte el 30 de abril de 2021. Desde entonces no recibe parches de seguridad, así que cualquier vulnerabilidad posterior queda permanentemente sin corregir. El SO es el componente más crítico de la pila.

**Tipo de solución:** Remediación definitiva (actualizar a una versión con soporte).

**Pasos técnicos**
- Snapshot completo de la VM antes de intervenir.
- `lsb_release -a` para verificar la versión.
- Evaluar `sudo do-release-upgrade` hacia 18.04; si hay dependencias incompatibles, migrar.
- **Opción preferida:** provisionar una instancia con Ubuntu 22.04/24.04 LTS, migrar servicios (Apache, FTP, SSH) y dar de baja la 16.04.
- `sudo apt update && sudo apt upgrade -y`
- `sudo apt install unattended-upgrades && sudo dpkg-reconfigure unattended-upgrades`

**Validación:** `lsb_release -a` confirma 22.04 LTS o posterior; repetir el escaneo con Nessus y verificar que el plugin 201351 ya no reporta la condición crítica.

**Recursos:** 4–8 h · administrador de sistemas senior con root · repositorios oficiales + snapshot previo.

**Riesgo residual:** Bajo. Ubuntu 22.04 LTS tiene soporte hasta 2027 (2032 con ESM), sujeto a gestión continua de parches.
