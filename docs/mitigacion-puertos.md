# Mitigación de puertos y servicios detectados con Nmap

## Objetivo del análisis

El objetivo del escaneo con Nmap ha sido identificar la superficie de exposición del servidor, comprobando qué puertos TCP se encuentran abiertos, qué servicios responden en ellos y qué versiones de software pueden ser detectadas externamente.

Reducir la superficie de ataque es una medida básica de bastionado. Cada puerto abierto representa un posible punto de entrada, por lo que únicamente deben permanecer accesibles aquellos servicios estrictamente necesarios para la función del servidor.

## Puertos detectados

Durante el escaneo se identificaron los servicios activos publicados por el servidor. Los resultados completos se encuentran documentados en el archivo:

`docs/escaneo-nmap.txt`

Los puertos habituales en este entorno son:

- `22/tcp`: servicio SSH para administración remota.
- `80/tcp`: servicio HTTP, si existe servidor web.
- `443/tcp`: servicio HTTPS, si existe servidor web con TLS.

## Análisis de riesgos

### SSH

El servicio SSH es necesario para la administración remota del servidor, pero también es uno de los objetivos más habituales en ataques de fuerza bruta. Si permanece expuesto, debe protegerse mediante varias medidas:

- Usar autenticación mediante clave pública siempre que sea posible.
- Deshabilitar el acceso directo del usuario `root`.
- Utilizar un puerto personalizado si la práctica o la infraestructura lo requiere.
- Aplicar reglas restrictivas en UFW.
- Activar limitación de intentos con `ufw limit`.
- Configurar Fail2ban para bloquear IPs con múltiples intentos fallidos.

### HTTP

El puerto `80/tcp` solo debe estar abierto si el servidor ofrece una aplicación web o si se utiliza para redirigir tráfico hacia HTTPS. En un entorno endurecido, no debería exponer información sensible ni paneles de administración sin autenticación.

Como mitigación, se recomienda:

- Mantener el servidor web actualizado.
- Redirigir HTTP hacia HTTPS.
- No mostrar versiones del servidor en cabeceras.
- Eliminar páginas por defecto de Apache o Nginx.
- Limitar el acceso a rutas administrativas.

### HTTPS

El puerto `443/tcp` debe utilizarse para tráfico web cifrado. Es preferible a HTTP porque protege la confidencialidad e integridad de las comunicaciones.

Como mitigación, se recomienda:

- Usar certificados TLS válidos.
- Deshabilitar protocolos antiguos.
- Mantener actualizadas las librerías criptográficas.
- Configurar cabeceras de seguridad en el servidor web.

## Servicios que deberían cerrarse

Debe cerrarse cualquier puerto que no sea necesario para la finalidad del servidor. En esta práctica, como criterio general:

- Si el servidor solo se administra por SSH, únicamente debería quedar abierto SSH.
- Si además aloja una web, deberían quedar abiertos SSH, HTTP y HTTPS.
- Cualquier otro servicio detectado por Nmap debe revisarse y, si no tiene justificación, deshabilitarse con `systemctl disable --now`.

Ejemplo de deshabilitación de un servicio innecesario:

`sudo systemctl disable --now nombre-del-servicio`

## Mitigación de versiones antiguas

Si Nmap detecta versiones antiguas de servicios, la mitigación principal consiste en actualizar los paquetes del sistema:

`sudo apt update && sudo apt upgrade -y`

Después de actualizar, debe repetirse el escaneo para comprobar si la versión expuesta ha cambiado o si el servicio sigue siendo vulnerable.

También se recomienda revisar la configuración del servicio afectado y eliminar banners o cabeceras que expongan información innecesaria sobre versiones concretas.

## Conclusión

El análisis con Nmap permite conocer qué servicios están expuestos en el servidor y sirve como base para aplicar medidas de reducción de superficie de ataque. La mitigación principal consiste en cerrar todo servicio innecesario, restringir el acceso mediante UFW, actualizar versiones antiguas y aplicar controles adicionales como Fail2ban para proteger servicios críticos como SSH.
