# Plan 7 · TCP Timestamps habilitados

**Severidad:** Baja — CVSS 2.6 · **Fuente:** OpenVAS

| Campo | Detalle |
|---|---|
| CWE | CWE-200: Exposure of Sensitive Information |
| Host / puerto | `<VICTIMA>` — general/tcp |
| Marco normativo | ISO/IEC 27002:2022 Control 8.8 · NIST CSF 2.0 PR.DS-2 |

**Contexto:** el servidor responde con marcas de tiempo TCP, lo que permite estimar su tiempo de actividad (uptime) y, en teoría, apoyar otras técnicas de reconocimiento. Riesgo muy bajo.

**Tipo de solución:** Mitigación (deshabilitar timestamps si no se requieren).

**Pasos técnicos**
- Temporal: `sudo sysctl -w net.ipv4.tcp_timestamps=0`
- Persistente: agregar `net.ipv4.tcp_timestamps = 0` en `/etc/sysctl.conf` y `sudo sysctl -p`

**Validación:** `nmap -O <VICTIMA>` ya no debe estimar uptime por timestamps; repetir el escaneo con OpenVAS.

**Recursos:** 10–15 min · sysadmin.

**Riesgo residual:** Nulo en términos de seguridad. Deshabilitar timestamps puede afectar levemente el rendimiento TCP en redes de alta latencia (PAWS/RTT).
