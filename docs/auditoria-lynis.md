# Auditoría automatizada de seguridad con Lynis

## Objetivo

El objetivo de esta fase ha sido realizar una auditoría automatizada de seguridad del servidor Linux mediante Lynis. Esta herramienta analiza la configuración del sistema operativo, identifica debilidades de seguridad, genera advertencias y propone recomendaciones de bastionado.

## Auditoría inicial

La primera auditoría se ejecutó con:

`sudo lynis audit system`

La salida completa se guardó en:

`docs/lynis-audit-inicial.txt`

También se conservaron los reportes generados por Lynis:

- `docs/lynis-inicial.log`
- `docs/lynis-report-inicial.dat`

El Hardening Index inicial obtenido fue:

`62`

## Advertencias y sugerencias iniciales

Las advertencias detectadas inicialmente se guardaron en:

`docs/lynis-warnings-inicial.txt`

Las sugerencias detectadas inicialmente se guardaron en:

`docs/lynis-suggestions-inicial.txt`

Estas recomendaciones sirvieron como base para aplicar mejoras de bastionado en el sistema.

## Mitigaciones aplicadas

### 1. Banner legal para SSH

Se configuró un banner de advertencia en SSH mediante el archivo:

`/etc/issue.net`

Además, se añadió una configuración específica en:

`/etc/ssh/sshd_config.d/20-hardening.conf`

Con esta medida se informa de que el acceso está restringido a personal autorizado y que la actividad puede ser registrada. También se reforzó SSH deshabilitando el acceso directo como root, desactivando X11 forwarding y limitando los intentos máximos de autenticación.

### 2. Política de calidad de contraseñas

Se instaló y configuró `libpam-pwquality` para exigir contraseñas más robustas en futuros cambios de contraseña.

La configuración aplicada establece una longitud mínima de 12 caracteres y exige el uso de distintos tipos de caracteres, reduciendo el riesgo de contraseñas débiles.

La evidencia se guardó en:

- `configs/lynis/pwquality.conf.before`
- `configs/lynis/pwquality.conf.after`

### 3. Activación de auditd

Se instaló y activó `auditd`, que permite registrar eventos de seguridad relevantes del sistema.

El estado del servicio se guardó en:

`docs/auditd-status.txt`

Esta medida mejora la capacidad de auditoría y trazabilidad ante incidentes.

### 4. Restricción de compiladores

Se revisaron los permisos de compiladores como `gcc`, `g++`, `cc` y `make`. En caso de existir, se asignaron al grupo `compilers` y se restringió su uso mediante permisos `750`.

Esta medida reduce el riesgo de que un usuario no autorizado compile herramientas maliciosas directamente en el servidor.

Las evidencias se guardaron en:

- `docs/compiler-permissions-before.txt`
- `docs/compiler-permissions-after.txt`

## Auditoría final

Tras aplicar las mitigaciones, se ejecutó de nuevo:

`sudo lynis audit system`

La salida completa se guardó en:

`docs/lynis-audit-final.txt`

También se conservaron los reportes finales:

- `docs/lynis-final.log`
- `docs/lynis-report-final.dat`

El Hardening Index final obtenido fue:

`65`

## Comparación de resultados

| Auditoría | Hardening Index |
|---|---:|
| Inicial | 62 |
| Final | 65 |

## Conclusión

Lynis permitió identificar debilidades de seguridad y aplicar mejoras concretas sobre el servidor. Las mitigaciones aplicadas refuerzan el acceso SSH, mejoran la política de contraseñas, añaden capacidad de auditoría mediante `auditd` y reducen riesgos asociados al uso de compiladores.

La mejora del Hardening Index final refleja que el servidor tiene un nivel de bastionado superior tras aplicar las recomendaciones.
