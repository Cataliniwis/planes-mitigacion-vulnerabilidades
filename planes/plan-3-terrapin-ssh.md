# Plan 3 · Debilidad Terrapin en SSH (CVE-2023-48795)

**Severidad:** Media — CVSS 5.9 · **Fuente:** Nessus Essentials (Plugin 187126)

| Campo | Detalle |
|---|---|
| CVE | CVE-2023-48795 |
| CWE | CWE-354: Improper Validation of Integrity Check Value |
| Host / puerto | `<VICTIMA>` — 22/tcp |
| Marco normativo | ISO/IEC 27002:2022 Control 8.20 / 8.24 · CIS v8.1 Control 12 · NIST CSF 2.0 PR.DS-2 |

**Contexto:** Terrapin es un ataque de truncamiento de prefijo en el handshake SSH. Un atacante con posición de *man-in-the-middle* puede eliminar mensajes de extensión negociados, degradando la seguridad. Afecta a `chacha20-poly1305@openssh.com` y a modos CBC con ETM.

**Tipo de solución:** Mitigación (actualizar OpenSSH a una versión con el parche).

**Pasos técnicos**
- `ssh -V` para ver la versión.
- `sudo apt update && sudo apt install openssh-server` (OpenSSH 9.6+).
- Si no hay actualización disponible (SO EOL), mitigar en `/etc/ssh/sshd_config`:
  - `Ciphers -chacha20-poly1305@openssh.com`
  - `MACs -hmac-sha2-256-etm@openssh.com,-hmac-sha2-512-etm@openssh.com`
- `sudo systemctl restart sshd`
- `sshd -T | grep -E 'ciphers|macs'`

**Validación:** `ssh -Q cipher localhost` no debe listar `chacha20-poly1305`; repetir el escaneo con Nessus.

**Recursos:** 30–60 min · sysadmin con root. La solución de fondo es actualizar el SO (Plan 1).

**Riesgo residual:** Bajo tras actualizar OpenSSH; impacto operacional mínimo.
