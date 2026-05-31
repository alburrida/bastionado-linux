# Fundamentos de criptografía y defensa

## Criptografía simétrica y asimétrica

La criptografía simétrica utiliza una única clave compartida para cifrar y descifrar información. Es rápida y eficiente, por lo que se usa habitualmente para proteger grandes volúmenes de datos, como discos, copias de seguridad, comunicaciones internas o archivos sensibles. Su principal inconveniente es la distribución segura de la clave: si una tercera persona obtiene esa clave, podrá descifrar la información protegida con ella.

Uno de los algoritmos simétricos más recomendados actualmente es AES, especialmente AES-128, AES-192 y AES-256. AES es un estándar criptográfico aprobado para proteger datos electrónicos y sigue siendo una referencia para cifrado simétrico moderno.

La criptografía asimétrica utiliza dos claves diferentes: una clave pública y una clave privada. La clave pública puede compartirse libremente, mientras que la clave privada debe mantenerse protegida. Este modelo permite cifrar información para un destinatario concreto y también crear firmas digitales. Es más lenta que la criptografía simétrica, por lo que suele emplearse para intercambio de claves, autenticación y firma.

Entre los algoritmos asimétricos actuales destacan RSA con longitudes de clave suficientes, como 3072 o 4096 bits, y algoritmos basados en curvas elípticas, como ECDSA o EdDSA. En esta práctica se utiliza RSA de 4096 bits mediante GPG porque es una opción robusta, compatible y ampliamente soportada.

## Funciones hash criptográficas

Una función hash criptográfica transforma una entrada de tamaño variable en una salida de tamaño fijo llamada resumen, huella o digest. No está diseñada para ser reversible. Su objetivo no es ocultar información para recuperarla después, sino generar una representación única que permita comprobar integridad o comparar datos.

Una función hash criptográfica debe cumplir varias propiedades:

- Resistencia a preimagen: no debe ser viable obtener el dato original a partir del hash.
- Resistencia a segunda preimagen: no debe ser viable encontrar otra entrada distinta que produzca el mismo hash.
- Resistencia a colisiones: no debe ser viable encontrar dos entradas diferentes con el mismo resultado.
- Efecto avalancha: un pequeño cambio en la entrada debe producir un resultado completamente diferente.

Actualmente se consideran recomendables familias como SHA-2 y SHA-3. En cambio, MD5 y SHA-1 se consideran inseguros para usos criptográficos modernos. SHA-1 debe ser abandonado en favor de SHA-2 o SHA-3, y MD5 no debe utilizarse para integridad criptográfica ni firmas digitales.

## Cifrado, hashing y codificación

El cifrado protege la confidencialidad. Convierte información legible en información cifrada usando una clave. Si se posee la clave correcta, el contenido puede recuperarse. Un ejemplo sería cifrar un archivo `secrets.json` con GPG para que solo pueda leerlo quien tenga la clave privada correspondiente.

El hashing genera una huella de los datos, pero no permite recuperar el contenido original. Sirve para comprobar integridad o almacenar verificadores de contraseñas usando algoritmos adecuados. Un ejemplo sería calcular el hash de un archivo para comprobar que no se ha modificado.

La codificación no es una medida de seguridad. Solo transforma datos de un formato a otro para facilitar su almacenamiento o transmisión. Un ejemplo sería Base64, que convierte datos binarios en texto, pero cualquier persona puede decodificarlo si conoce el formato.
