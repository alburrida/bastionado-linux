# Verificación de cifrado y firma con GPG

## Clave utilizada

Se generó un par de claves GPG con algoritmo RSA de 4096 bits usando una identidad ficticia para la práctica:

`Alumno ASIR <asir-practica@example.local>`

La clave pública fue exportada en formato ASCII al archivo:

`docs/asir-public-key.asc`

## Cifrado

Se creó un archivo ficticio `secrets.json` con credenciales de prueba. Posteriormente se cifró usando la clave pública del usuario de práctica, generando el archivo:

`secrets.json.gpg`

Después del cifrado, el archivo original sin cifrar fue eliminado mediante:

`shred -u secrets.json`

La comprobación de descifrado se realizó con:

`gpg --decrypt secrets.json.gpg`

El resultado permitió recuperar correctamente el contenido original, confirmando que el cifrado y descifrado funcionan correctamente.

## Firma digital

Se creó el archivo:

`docs/verificacion.txt`

Después se firmó digitalmente con firma integrada mediante:

`gpg --local-user "asir-practica@example.local" --clearsign docs/verificacion.txt`

La firma generada se verificó con:

`gpg --verify docs/verificacion.txt.asc`

La salida de verificación se guardó en:

`docs/gpg-firma-verify.txt`

El resultado confirmó una firma válida para la identidad ficticia de la práctica:

`Good signature from "Alumno ASIR <asir-practica@example.local>"`

La advertencia sobre firma no separada es normal, ya que se utilizó `--clearsign`. En este formato, el contenido firmado está integrado dentro del archivo `.asc`.
