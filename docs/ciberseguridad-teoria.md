# Ciberseguridad y defensa en profundidad

La ciberseguridad debe abordarse mediante una estrategia de defensa en profundidad porque ningún mecanismo de protección es suficiente por sí solo. Un servidor puede tener contraseñas seguras, pero seguir siendo vulnerable si expone servicios innecesarios, si no registra eventos de seguridad, si no limita los intentos de acceso o si no dispone de mecanismos para detectar modificaciones sospechosas en el sistema.

La defensa en profundidad consiste en aplicar varias capas de seguridad complementarias. En este entorno se combinan medidas preventivas, como el bastionado del kernel y la reducción de servicios activos; medidas de control de acceso, como el cortafuegos UFW; medidas criptográficas, como el cifrado y la firma digital mediante GPG; medidas de detección, como AIDE; medidas de respuesta automática, como Fail2ban; y medidas de auditoría, como Lynis.

Este enfoque reduce el riesgo porque obliga a un atacante a superar varias barreras independientes. Si una capa falla, otra puede limitar el impacto. Por ejemplo, aunque un servicio SSH esté expuesto, UFW puede limitar el tráfico permitido, Fail2ban puede bloquear intentos de fuerza bruta y los registros del sistema pueden permitir analizar la actividad sospechosa. Del mismo modo, si un atacante modifica archivos críticos del sistema, AIDE puede detectar cambios no autorizados comparando el estado actual con una base de datos de integridad generada previamente.

Por tanto, la seguridad de un servidor no debe basarse únicamente en instalar herramientas, sino en configurar correctamente el sistema, reducir la superficie de ataque, aplicar controles de acceso, proteger la información sensible y mantener evidencias que permitan auditar el estado de seguridad.
