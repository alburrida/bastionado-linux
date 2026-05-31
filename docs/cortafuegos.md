# Cortafuegos de red con UFW

## Objetivo

El objetivo de esta fase ha sido configurar un cortafuegos local mediante UFW para reducir la superficie de exposición del servidor. La política aplicada sigue un criterio restrictivo: denegar por defecto todo el tráfico entrante y permitir el tráfico saliente.

Esta configuración mejora la seguridad porque impide conexiones externas no autorizadas hacia servicios que puedan estar activos en el sistema, permitiendo únicamente aquellos puertos necesarios para la administración y funcionamiento del servidor.

## Estado inicial

Antes de aplicar la configuración se comprobó el estado inicial del cortafuegos con:

`sudo ufw status verbose`

La salida fue guardada en:

`docs/ufw-status-inicial.txt`

## Políticas por defecto aplicadas

Se configuraron las siguientes políticas por defecto:

`sudo ufw default deny incoming`

`sudo ufw default allow outgoing`

La primera regla deniega todo el tráfico entrante no permitido expresamente. La segunda permite que el servidor pueda iniciar conexiones salientes, por ejemplo para actualizaciones del sistema o resolución DNS.

## Reglas permitidas

Se permitió el acceso SSH antes de habilitar el cortafuegos para evitar perder acceso remoto al servidor:

`sudo ufw allow 22/tcp comment 'Permitir SSH'`

Además, se añadió una regla con limitación de intentos para mitigar ataques de fuerza bruta contra SSH:

`sudo ufw limit 22/tcp comment 'Limitar intentos SSH'`

También se permitió tráfico web entrante por HTTP y HTTPS:

`sudo ufw allow 80/tcp comment 'Permitir HTTP'`

`sudo ufw allow 443/tcp comment 'Permitir HTTPS'`

## Activación del cortafuegos

El cortafuegos fue habilitado con:

`sudo ufw enable`

El estado final de reglas activas se comprobó con:

`sudo ufw status verbose`

La salida fue guardada en:

`docs/ufw-status-final.txt`

## Logging de UFW

El registro de eventos del cortafuegos fue activado con:

`sudo ufw logging on`

Los eventos de bloqueo de UFW se registran habitualmente en:

`/var/log/ufw.log`

También pueden consultarse mediante el journal del sistema con:

`sudo journalctl -u ufw`

## Interpretación de logs de bloqueo

Una línea de bloqueo de UFW puede contener campos como `SRC`, `DST`, `SPT` y `DPT`.

- `SRC`: dirección IP de origen del paquete.
- `DST`: dirección IP de destino del paquete.
- `SPT`: puerto de origen desde el que se generó la conexión.
- `DPT`: puerto de destino al que se intentó conectar.

Ejemplo interpretativo:

Si aparece `SRC=192.168.1.50 DST=192.168.1.100 SPT=51544 DPT=22`, significa que el equipo `192.168.1.50` intentó conectarse desde el puerto origen `51544` al puerto destino `22` del servidor `192.168.1.100`.

## Comprobación de reglas activas

Las reglas activas pueden comprobarse con:

`sudo ufw status verbose`

Para verlas numeradas:

`sudo ufw status numbered`

Esto permite identificar reglas concretas y eliminarlas si fuera necesario mediante:

`sudo ufw delete <numero>`

## Conclusión

La configuración de UFW limita el tráfico entrante a los servicios estrictamente necesarios: SSH, HTTP y HTTPS. Esta medida reduce la superficie de ataque y complementa otras capas de seguridad como Fail2ban, auditoría de servicios, bastionado del kernel y análisis de puertos mediante Nmap.
