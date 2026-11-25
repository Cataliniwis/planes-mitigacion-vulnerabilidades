# Plan 5 · Transmisión en texto claro — HTTP sin TLS

**Severidad:** Media — CVSS 4.8 · **Fuente:** OpenVAS

| Campo | Detalle |
|---|---|
| CVE | Sin CVE específico (OWASP A02:2021) |
| CWE | CWE-319: Cleartext Transmission of Sensitive Information |
| Host / puerto | `<VICTIMA>` — 80/tcp (ruta `/login_2/` con Basic Auth) |
| Marco normativo | ISO/IEC 27002:2022 Control 8.24 / 5.14 · CIS v8.1 Control 3.10 · NIST CSF 2.0 PR.DS-2 · OWASP A02:2021 |

**Contexto:** `/login_2/` expone un formulario HTTP Basic sobre conexión sin cifrar. Un atacante en posición de *man-in-the-middle* (p. ej. ARP poisoning en LAN) puede capturar las credenciales en texto plano.

**Tipo de solución:** Remediación (HTTPS con TLS y redirección de HTTP a HTTPS).

**Pasos técnicos**
- `sudo a2enmod ssl && sudo a2enmod rewrite`
- Certificado (en producción, Let's Encrypt o CA corporativa):
  `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache.key -out /etc/ssl/certs/apache.crt`
- Configurar el VirtualHost SSL (`default-ssl.conf`) y habilitarlo: `sudo a2ensite default-ssl`
- Redirección en `000-default.conf`: `Redirect permanent / https://<VICTIMA>/`
- `sudo systemctl reload apache2`
- Cabecera HSTS: `Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"`

**Validación:** acceder por HTTP debe redirigir a HTTPS; capturar con Wireshark y confirmar que las credenciales ya no viajan en claro; repetir el escaneo con OpenVAS.

**Recursos:** 1–2 h · sysadmin / DevOps.

**Riesgo residual:** Bajo con certificado válido; con certificado autofirmado, advertencias del navegador (aceptable en laboratorio).
