# Plan 2 · RCE en la funcionalidad `/ping/` (OS Command Injection)

**Severidad:** Alta — CVSS 7.5 · **Fuente:** OpenVAS (CS Whois Lookup, BID 34700)

| Campo | Detalle |
|---|---|
| CVE | Sin CVE específico asignado |
| CWE | CWE-78: OS Command Injection |
| Host / puerto | `<VICTIMA>` — 80/tcp |
| Marco normativo | ISO/IEC 27002:2022 Control 8.28 · CIS v8.1 Control 16 · NIST CSF 2.0 PR.IP-2 / DE.CM-8 · OWASP A03:2021 |

**Contexto:** `/ping/index.php` pasa el parámetro `ip` directamente a una llamada del sistema operativo sin sanear. Un atacante no autenticado puede inyectar comandos con `;`, `&&` o `|`. OpenVAS lo confirmó y fue **explotado manualmente en el Desafío 1**.

**Tipo de solución:** Remediación a nivel de código.

**Pasos técnicos**
- Localizar el archivo: `find /var/www/html -name 'index.php' -path '*/ping/*'`
- **Opción A (recomendada):** eliminar la funcionalidad si no se usa: `sudo rm -rf /var/www/html/ping/`
- **Opción B:** reescribir con validación estricta:
  ```php
  $ip = filter_var($_GET['ip'], FILTER_VALIDATE_IP);
  if (!$ip) { die('IP inválida'); }
  $output = shell_exec('ping -c 1 ' . escapeshellarg($ip));
  ```
- Deshabilitar funciones peligrosas en `php.ini`: `disable_functions = exec,passthru,shell_exec,system,proc_open,popen`
- `sudo systemctl reload apache2`
- WAF: `sudo apt install libapache2-mod-security2`

**Validación:** intentar `.../ping/index.php?ip=;id` y verificar que no devuelve salida del sistema; repetir el escaneo con OpenVAS.

**Recursos:** 1–2 h · desarrollador web o sysadmin con acceso al código.

**Riesgo residual:** Muy bajo con la Opción A; bajo con la Opción B según la calidad de la validación. Revisar el resto de la aplicación por otros puntos de inyección.
