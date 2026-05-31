# Detección de intrusos e integridad con AIDE

## Objetivo

El objetivo de esta fase ha sido configurar AIDE como sistema de detección de intrusos basado en integridad de archivos. AIDE permite generar una base de datos inicial con el estado legítimo del sistema y compararla posteriormente con el estado real del servidor para detectar modificaciones, eliminaciones o archivos nuevos sospechosos.

## Funcionamiento de AIDE

AIDE analiza archivos y directorios del sistema y registra atributos como permisos, propietario, grupo, tamaño, fechas y hashes criptográficos. Esta información se almacena en una base de datos de referencia.

Cuando se ejecuta una comprobación posterior, AIDE compara el estado actual del sistema con la base de datos inicial. Si detecta diferencias, las muestra en un reporte. Esto permite identificar cambios no autorizados en archivos críticos del sistema.

## Configuración utilizada

La ejecución de AIDE con la configuración completa del sistema resultaba demasiado lenta para el entorno de práctica, ya que analizaba una gran cantidad de rutas del servidor.

Para realizar una verificación controlada, se creó una configuración mínima en:

`configs/aide/aide-practica.conf`

Esta configuración vigila específicamente los dos elementos requeridos en el ejercicio:

- `/etc/passwd`
- `/usr/bin/backdoor`

## Simulación de intrusión

Para comprobar el funcionamiento de AIDE se simuló una brecha de seguridad mediante dos acciones:

1. Creación de un archivo sospechoso en `/usr/bin/backdoor`.
2. Modificación controlada del archivo `/etc/passwd`.

Después se ejecutó una comprobación de integridad con AIDE y el resultado se guardó en:

`docs/reporte-integridad.txt`

El reporte mostró correctamente:

- Una entrada añadida: `/usr/bin/backdoor`.
- Una entrada modificada: `/etc/passwd`.

## Resultado obtenido

AIDE detectó diferencias entre la base de datos de referencia y el sistema de archivos actual.

El resumen del reporte indicó:

- `Added entries: 1`
- `Changed entries: 1`

La entrada añadida fue:

`/usr/bin/backdoor`

La entrada modificada fue:

`/etc/passwd`

Esto confirma que AIDE detectó correctamente la intrusión simulada.

## Protección de la base de datos de AIDE

Es fundamental proteger la base de datos de firmas de AIDE fuera del propio servidor auditado porque esa base representa el estado considerado legítimo del sistema. Si un atacante obtiene privilegios de administrador en el servidor, podría modificar archivos maliciosamente y después regenerar o alterar la base de datos de AIDE para ocultar sus cambios.

Por ese motivo, la base de datos de integridad debe almacenarse preferiblemente en un medio externo, de solo lectura o en un sistema separado. De esta forma, aunque el servidor sea comprometido, el atacante no podrá manipular fácilmente la referencia usada para detectar alteraciones.

Guardar la base de datos en una ubicación externa mejora la fiabilidad de la detección, porque permite comparar el sistema comprometido contra una referencia limpia y confiable.

## Restauración del sistema

Tras generar el reporte de integridad, se restauró `/etc/passwd` desde una copia de seguridad previa y se eliminó el archivo `/usr/bin/backdoor`, dejando el sistema de nuevo en estado limpio.

## Conclusión

AIDE aporta una capa de detección dentro de una estrategia de defensa en profundidad. No impide por sí mismo una intrusión, pero permite descubrir modificaciones sospechosas en archivos críticos, binarios del sistema o rutas sensibles. Combinado con medidas preventivas como UFW, Fail2ban y hardening del kernel, mejora la capacidad de detección y respuesta ante incidentes.
