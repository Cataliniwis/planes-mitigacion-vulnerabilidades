# Plan 6 · Algoritmos MAC débiles en SSH

**Severidad:** Baja — CVSS 2.6 · **Fuente:** OpenVAS

| Campo | Detalle |
|---|---|
| CWE | CWE-327: Use of a Broken or Risky Cryptographic Algorithm |
| Host / puerto | `<VICTIMA>` — 22/tcp |
| Marco normativo | ISO/IEC 27002:2022 Control 8.24 · CIS v8.1 Control 3.10 · NIST CSF 2.0 PR.DS-2 |

**Contexto:** el servicio SSH admite algoritmos MAC basados en HMAC-SHA1 y/o MAC de 64 bits, más débiles que las alternativas modernas.

**Tipo de solución:** Mitigación (restringir a MAC fuertes).

**Pasos técnicos**
- En `/etc/ssh/sshd_config`:
  `MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512,hmac-sha2-256`
- `sudo systemctl restart sshd`
- `sshd -T | grep macs`

**Validación:** `nmap -p 22 --script ssh2-enum-algos <VICTIMA>` no debe listar MAC débiles; repetir el escaneo con OpenVAS.

**Recursos:** 15–30 min · sysadmin.

**Riesgo residual:** Mínimo. Verificar compatibilidad con clientes antiguos antes de aplicar.
