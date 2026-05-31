# Protección contra fuerza bruta con Fail2ban

## Objetivo

El objetivo de esta fase ha sido configurar Fail2ban para proteger el servicio SSH frente a ataques de fuerza bruta. Fail2ban analiza los registros del sistema y bloquea automáticamente direcciones IP que generan demasiados intentos fallidos de autenticación.

## Instalación

Fail2ban fue instalado mediante el gestor de paquetes del sistema:

`sudo apt install -y fail2ban`

Después se habilitó e inició el servicio con:

`sudo systemctl enable --now fail2ban`

## Configuración aplicada

Se creó una copia de la configuración base:

`sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`

La sección `[sshd]` fue configurada con los siguientes parámetros:

- `enabled = true`: activa la protección para SSH.
- `port = 22`: indica el puerto SSH interno del servidor.
- `filter = sshd`: utiliza el filtro de Fail2ban para analizar intentos SSH fallidos.
- `backend = systemd`: obtiene los eventos desde el journal de systemd.
- `bantime = 1h`: bloquea la IP atacante durante una hora.
- `findtime = 10m`: analiza los intentos fallidos dentro de una ventana de diez minutos.
- `maxretry = 5`: bloquea la IP tras cinco intentos fallidos.

La configuración aplicada se guardó también en:

`configs/fail2ban/jail.local`

## Comprobación del servicio

El estado del servicio se verificó mediante:

`sudo systemctl status fail2ban`

También se comprobó el estado general de Fail2ban con:

`sudo fail2ban-client status`

Y el estado concreto del jail SSH con:

`sudo fail2ban-client status sshd`

Las salidas se documentaron en los archivos:

- `docs/fail2ban-service-status.txt`
- `docs/fail2ban-status-general.txt`
- `docs/fail2ban-status-sshd-inicial.txt`

## Simulación de ataque

Para comprobar el funcionamiento, se simularon varios intentos fallidos de inicio de sesión SSH desde otra terminal usando un usuario inexistente. Tras superar el límite configurado, Fail2ban añadió la IP sospechosa a la lista de baneos activos.

La comprobación se realizó con:

`sudo fail2ban-client status sshd`

La salida del estado con IP baneada se guardó en:

`docs/fail2ban-status-sshd-baneado.txt`

## Desbaneo seguro

Una vez verificado el bloqueo, la IP fue desbaneada de forma manual con:

`sudo fail2ban-client set sshd unbanip <IP_BANEADA>`

La salida se guardó en:

`docs/fail2ban-unban.txt`

## Conclusión

Fail2ban añade una capa de respuesta automática frente a ataques de fuerza bruta. Combinado con UFW, permite reducir el riesgo de accesos no autorizados al servicio SSH y limita la capacidad de un atacante para realizar intentos repetidos de autenticación.
