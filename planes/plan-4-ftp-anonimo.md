# Plan 4 · FTP anónimo habilitado

**Severidad:** Media — CVSS 6.4 · **Fuente:** OpenVAS

| Campo | Detalle |
|---|---|
| CVE | CVE-1999-0497 (configuración) |
| CWE | CWE-306: Missing Authentication for Critical Function |
| Host / puerto | `<VICTIMA>` — 21/tcp |
| Marco normativo | ISO/IEC 27002:2022 Control 8.3 / 5.17 · CIS v8.1 Control 6 · NIST CSF 2.0 PR.AC-1 |

**Contexto:** el servidor FTP permite iniciar sesión como `anonymous`/`ftp`. OpenVAS encontró un archivo accesible sin autenticación. FTP además transmite en texto claro.

**Tipo de solución:** Mitigación (deshabilitar el acceso anónimo); remediación: reemplazar por SFTP/FTPS.

**Pasos técnicos**
- `which vsftpd proftpd pure-ftpd`
- vsftpd: en `/etc/vsftpd.conf` -> `anonymous_enable=NO`
- ProFTPD: denegar el bloque `<Anonymous>` (`<Limit LOGIN> DenyAll </Limit>`)
- `sudo systemctl restart vsftpd`
- Si no se necesita FTP: `sudo systemctl disable --now vsftpd`
- Para transferencias, usar SFTP (OpenSSH) o FTPS.

**Validación:** `ftp <VICTIMA>` con credenciales anónimas debe ser rechazado; repetir el escaneo con OpenVAS.

**Recursos:** 15–30 min · sysadmin.

**Riesgo residual:** Nulo para acceso anónimo tras la mitigación; si se mantiene FTP con credenciales, persiste el texto claro (se resuelve con SFTP/FTPS).
