# Bastionado de servidores Linux

# Bastionado de servidores Linux

Práctica de seguridad defensiva sobre un servidor Linux basada en una estrategia de defensa en profundidad.
El proyecto incluye hardening inicial del sistema, configuración de cortafuegos, protección frente a fuerza bruta, cifrado y firma digital con GPG, análisis de puertos con Nmap, detección de cambios con AIDE y auditoría automatizada con Lynis.

## Objetivo

El objetivo de esta práctica es aplicar varias capas de seguridad sobre un servidor Ubuntu Server, reduciendo la superficie de ataque y dejando evidencias técnicas de las medidas aplicadas.

La seguridad no se basa en una única herramienta, sino en combinar controles preventivos, controles de detección, registros de auditoría y mecanismos de respuesta ante comportamientos sospechosos.

## Tecnologías utilizadas

* Ubuntu Server
* UFW
* GPG
* Nmap
* Fail2ban
* AIDE
* Lynis
* systemd
* sysctl
* Git

## Medidas implementadas

### Bastionado inicial

Se revisaron los servicios activos del sistema y se aplicaron parámetros de hardening del kernel mediante `sysctl`.

Archivo principal:

```text
configs/99-security.conf
```

Evidencias:

```text
docs/servicios-activos.txt
docs/servicios-activos-despues.txt
docs/puertos-activos.txt
docs/sysctl-output.txt
docs/sysctl-check.txt
```

### Criptografía y GPG

Se generó un par de claves GPG RSA de 4096 bits usando una identidad ficticia para la práctica.
También se cifró un archivo sensible ficticio y se creó una firma digital con verificación posterior.

Archivos principales:

```text
docs/asir-public-key.asc
secrets.json.gpg
docs/verificacion.txt
docs/verificacion.txt.asc
docs/gpg-firma-verify.txt
docs/gpg-verificacion.md
```

### Análisis de red con Nmap

Se realizaron escaneos TCP básicos, escaneos avanzados con detección de versiones y sistema operativo, y escaneos con scripts por defecto.

Evidencias:

```text
docs/nmap-basico.txt
docs/nmap-avanzado.txt
docs/nmap-scripts.txt
docs/escaneo-nmap.txt
docs/mitigacion-puertos.md
```

### Cortafuegos con UFW

Se configuró UFW con política restrictiva, denegando tráfico entrante por defecto y permitiendo únicamente los servicios necesarios.

Archivos y evidencias:

```text
docs/cortafuegos.md
docs/ufw-status-inicial.txt
docs/ufw-status-final.txt
configs/ufw/
```

### Protección contra fuerza bruta con Fail2ban

Se configuró Fail2ban para proteger el servicio SSH frente a intentos repetidos de autenticación fallida.

Archivos principales:

```text
configs/fail2ban/jail.local
docs/fail2ban.md
docs/fail2ban-service-status.txt
docs/fail2ban-status-general.txt
docs/fail2ban-status-sshd-inicial.txt
docs/fail2ban-status-sshd-baneado.txt
```

### Integridad del sistema con AIDE

Se utilizó AIDE para detectar cambios no autorizados en archivos del sistema.
La prueba simuló una intrusión creando un archivo sospechoso y modificando `/etc/passwd`.

Archivos principales:

```text
configs/aide/aide-practica.conf
docs/reporte-integridad.txt
docs/ids-explicacion.md
docs/aide-db-files.txt
```

El reporte muestra la detección de:

```text
/usr/bin/backdoor
/etc/passwd
```

### Auditoría con Lynis

Se ejecutó una auditoría inicial con Lynis, se aplicaron mejoras de hardening y se repitió la auditoría para comprobar la evolución del índice de bastionado.

Archivos principales:

```text
docs/auditoria-lynis.md
docs/lynis-audit-inicial.txt
docs/lynis-audit-final.txt
docs/lynis-report-inicial.dat
docs/lynis-report-final.dat
docs/lynis-hardening-index-inicial.txt
docs/lynis-hardening-index-final.txt
```

Mitigaciones aplicadas:

* Banner legal para SSH.
* Refuerzo de configuración SSH.
* Política de calidad de contraseñas con `libpam-pwquality`.
* Activación de `auditd`.
* Restricción de permisos sobre compiladores.

## Estructura del repositorio

```text
.
├── configs/
│   ├── 99-security.conf
│   ├── aide/
│   ├── fail2ban/
│   ├── lynis/
│   └── ufw/
├── docs/
│   ├── ciberseguridad-teoria.md
│   ├── criptografia.md
│   ├── escaneo-nmap.txt
│   ├── mitigacion-puertos.md
│   ├── cortafuegos.md
│   ├── fail2ban.md
│   ├── ids-explicacion.md
│   ├── auditoria-lynis.md
│   └── reporte-integridad.txt
├── secrets.json.gpg
├── RESUMEN-ENTREGA.md
├── README.md
└── .gitignore
```

## Documentación incluida

La carpeta `docs/` contiene la documentación técnica y las evidencias generadas durante la práctica.

Documentos principales:

```text
docs/ciberseguridad-teoria.md
docs/criptografia.md
docs/mitigacion-puertos.md
docs/cortafuegos.md
docs/fail2ban.md
docs/ids-explicacion.md
docs/auditoria-lynis.md
```

## Seguridad de la entrega

El repositorio no incluye claves privadas ni secretos en claro.

El archivo sensible ficticio se conserva únicamente en formato cifrado:

```text
secrets.json.gpg
```

La clave pública GPG se incluye porque forma parte de la práctica:

```text
docs/asir-public-key.asc
```

No se deben subir al repositorio:

```text
secrets.json
.gnupg/
private-keys-v1.d/
*.key
*.pem
*.sec
id_rsa
```

## Comandos útiles de revisión

Ver estructura del proyecto:

```bash
tree -a -L 4 -I ".git|__pycache__|venv"
```

Comprobar estado de Git:

```bash
git status
git log --oneline
```

Comprobar reglas UFW:

```bash
sudo ufw status verbose
```

Comprobar Fail2ban:

```bash
sudo fail2ban-client status sshd
```

Comprobar reporte AIDE:

```bash
cat docs/reporte-integridad.txt
```

Comprobar resumen de Lynis:

```bash
cat docs/auditoria-lynis.md
```

## Conclusión

La práctica demuestra la aplicación de una estrategia de defensa en profundidad sobre un servidor Linux.
Se han combinado medidas de reducción de superficie de ataque, bastionado del sistema, cifrado, control de acceso, detección de intrusiones, protección contra fuerza bruta y auditoría automatizada.

El resultado final es un servidor más controlado, documentado y preparado frente a amenazas comunes.
