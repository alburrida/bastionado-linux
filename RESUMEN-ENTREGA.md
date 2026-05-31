# Resumen de entrega

Esta práctica implementa una estrategia de defensa en profundidad sobre un servidor Ubuntu Server 22.04 LTS.

## Medidas aplicadas

- Bastionado inicial del sistema mediante configuración `sysctl`.
- Documentación teórica sobre defensa en profundidad.
- Documentación de criptografía simétrica, asimétrica, hashing y codificación.
- Generación de claves GPG RSA 4096 bits.
- Exportación de clave pública en `docs/asir-public-key.asc`.
- Cifrado de archivo sensible ficticio mediante GPG.
- Firma digital y verificación de integridad con GPG.
- Escaneo de superficie de ataque con Nmap.
- Configuración de cortafuegos UFW con política restrictiva.
- Protección SSH frente a fuerza bruta con Fail2ban.
- Detección de cambios no autorizados con AIDE.
- Auditoría automatizada con Lynis.
- Aplicación de mejoras de hardening tras auditoría inicial.

## Archivos principales

- `configs/99-security.conf`: configuración de hardening del kernel.
- `configs/fail2ban/jail.local`: configuración aplicada de Fail2ban.
- `configs/aide/aide-practica.conf`: configuración AIDE usada para la verificación controlada.
- `docs/reporte-integridad.txt`: reporte AIDE con detección de `/usr/bin/backdoor` y `/etc/passwd`.
- `docs/auditoria-lynis.md`: resumen de auditoría Lynis, mitigaciones e índices de hardening.
- `docs/escaneo-nmap.txt`: resultados de escaneo Nmap.
- `docs/cortafuegos.md`: documentación de UFW.
- `docs/criptografia.md`: fundamentos de criptografía.
- `docs/ciberseguridad-teoria.md`: justificación de defensa en profundidad.
