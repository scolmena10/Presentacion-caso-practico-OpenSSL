# Presentación caso práctico OpenSSL
## Cifrado Simétrico vs Asimétrico y Automatización con Scripts

> [!NOTE]
> ## Objetivo
> Nuestro proyecto tiene como objetivo comparar los algoritmos de cifrado simétrico y asimétrico, entender sus diferencias y ventaja. Lo haremos en su mayoria con la ayuda de scripts en Bash usando OpenSSL

> [!IMPORTANT]
> ## Integrantes
> - **Abdullah** – Conceptos y fundamentos del cifrado simétrico y asimétrico
> - **David** – Implementación práctica del cifrado simétrico AES-256-CBC mediante scripts
> - **Carlos** – Explicación teórica del cifrado asimétrico, claves y aplicaciones
> - **Santino** – Implementación práctica del cifrado asimétrico con RSA mediante scripts

---

# Introducción a la Criptografía
## Cifrado Simétrico
El Cifrado Simétrico es tipo de algoritmos criptográfico que usa la misma clave para cifrar como para descifrar información (La misma clave secreta que se usa para convertir el texto original que ha sido cifrado también se usa para volver a obtener el texto original)

### Características

- **Una única clave:** Se une para cifrar y descifrar la misma clave, lo positivo y negativo de esto es que, si tu y tu amigo compartes la misma clave, ambos podéis cifrar y descifrar los mensajes, pero si alguien adivina vuestra clave, podrá ver todos los mensajes
- **Rapidez y eficiencia:** Los algoritmos simétricos son rápidos y eficientes, por eso lo usan para transmisión de datos, almacenamiento cifrado y VPNs
- **Seguridad:** El cifrado no depende del algoritmo en sí, sino de mantener la clave secreta, si la clave es segura (larga, aleatoria, con símbolos…) El cifrado será muy complicado de romper, en cambio si es una palabra común será muy sencilla de averiguar
  
> [!WARNING]
> #### Problema Principal
> El gran problema de esto es que, ambas personas necesitan tener la misma clave secreta para poder comunicarse y antes de usarla uno se la tiene que enviar al otro, ahí está el problema. Si lo enviamos por WhatsApp, correo o redes, puede ser interceptada

> [!TIP]
> #### Solución
> Se puede usar otro tipo de cifrado llamado asimétrico, que no necesita compartir la misma clave, por ejemplo:
> Yo tengo una clave pública que la puede saber todo el mundo para cifrar los mensajes y una privada que sólo se yo, cualquiera puede usar mi clave pública para poder cifrar el mensaje y solo yo con la privada descifrarlo

## Implementación Técnica

### Cifrado Simétrico
Se utiliza AES-256-CBC en Ubuntu con OpenSSL

Creamos el directorio `cifrado_simetrico` en Ubuntu para agrupar las claves, mensajes y scripts

```
mkdir cifrado_simetrico
cd cifrado_simetrico
```
Creamos un archivo de texto llamado `mensaje.txt` con el comando 
```
echo "(mensaje para cifrar)" > mensaje.txt
```
y después comprobamos si se ha creado con el comando
```
cat mensaje.txt
Hola, esto es un mensaje secretoooo
```
Creamos el fichero con el comando
```
sudo nano encriptado.sh
```
Dentro crearemos el script que encriptará el mensaje
```bash
#!/bin/bash

openssl rand -hex 32 > clave_sim.hex
openssl enc -aes-256-cbc -pbkdf2 -salt -in mensaje.txt -out mensaje.enc -pass file:./clave_sim.hex

echo "Cifrado completado: mensaje.enc generado."
```
Daremos permisos y ejecutaremos el script
```
chmod +x encriptado.sh
./encriptado.sh
```

> [!NOTE]
> #### ¿Qué hace este script?
>
> - Genera una clave segura de 256 bits
> - Cifra el mensaje usando AES-256-CBC en mensaje.enc
> - Guarda la clave en clave_sim.hex

Creamos el fichero con el comando
```
sudo nano desencriptado.sh
```
Y agregaremos el script que desencriptará el mensaje

```bash
#!/bin/bash

openssl enc -d -aes-256-cbc -pbkdf2 -in mensaje.enc -out mensaje_desencriptado.txt -pass file:./clave_sim.hex

echo "Descifrado completado: se generó mensaje_desencriptado.txt"
```
Daremos permisos y ejecutaremos el script
```
chmod +x desencriptado.sh
./desencriptado.sh
```
Una vez ejecutado el script podremos ver el mensaje desencriptado
```
cat mensaje_desencriptado.txt
Hola, esto es un mensaje secretoooo
```
> [!NOTE]
> #### Estructura final del directorio
>
> cifrado_simetrico
> - mensaje.txt
> - clave_sim.hex
> - mensaje_desencriptado.txt
> - encriptado.sh
> - desencriptado.sh

---

## Cifrado Asimétrico
Los algoritmos asimétricos son un tipo de criptografía que usa dos claves diferentes relacionadas matemáticamente y usan dos claves una pública y otra privada
#### Tipos de claves:
- **Clave pública:** Se comparte libremente. Las que se cifran con la clave pública solo se pueden descifrar con la clave privada
- **Clave privada:** Se debe mantener protegida. Las que se cifran con la clave privada solo se pueden descifrar con la clave pública

### Características

- **Seguridad:** Tiene una mayor seguridad en la distribución de claves, ya que no hace falta revelar la clave privada
- Permite **firmas digitales**, certificados y autenticación, esto agrega integridad y no repudio
- **Rendimiento:** Los algoritmos asimétricos son mucho más lentos, costosos y menos adecuados para grandes volumenes

> [!TIP]
> ### Ejemplos de usos reales
> 
> - HTTPS y certificados SSL/TLS
> - Cifrar información sensible antes de enviarla
> - Firmas digitales en correos electrónicos o documentos
> - Implementar sistemas de autenticación sin contraseñas compartidas
> - Acceso remoto y autenticación

> [!NOTE]
> ### Ejemplo práctico
> - Santino usa la clave pública de la empresa para cifrar un mensaje, y solo la empresa con su clave privada puede descifrarlo

### Cifrado Asimétrico con RSA

Crearé un directorio llamado `presentación` para no dejar los archivos sueltos

```
mkdir presentacion
cd presentacion
```
Dentro del directorio `presentacion` crearé el script
```
sudo nano script1.sh
```
Generaré las claves, privada, la clave secreta y clave pública (la clave que se comparte)
```
#!/bin/bash

echo “Creando las dos claves”

openssl genrsa -out presentacion_privada.key 1024

openssl rsa -out presentacion_privada.key -pubout -out presentacion_publica.pem
```
Daremos permisos y ejecutaremos el script
```
chmod +x script1.sh
./script1.sh
```
> [!NOTE]
> #### ¿Qué hace este script?
>
> - Genera una clave privada RSA de 1024 bits en presentacion_privada.key
> - Crea la clave pública derivada de la clave privada (la cual puede compartirse) en presentacion_publica.pem
> - Deja listas ambas claves para el proceso de cifrado/descifrado

Cifraremos mensaje que necesitemos enviar con la clave pública `presentacion_publica.pem` con el comando
```bash
echo “HOLAAAAAA” | openssl pkeyutl -encrypt -pubin -inkey presentacion_publica.pem -out mensaje.dat
```
Una vez tengamos el mensaje cifrado lo podremos descifrar con la clave privada `presentacion_privada.key` con el comando
```bash
openssl pkeyutl -decrypt -inkey presentacion_privada.key -in mensaje.dat
```
Una vez ejecutado el script podremos ver el mensaje desencriptado
```
cat mensaje.dat
HOLAAAAAA
```
> [!NOTE]
> #### Estructura final del directorio
>
> presentacion
> - presentacion_privada.key
> - presentacion_publica.pem
> - script1.sh
> - mensaje.dat
---
# Conclusión
Los algoritmos **simétricos y asimétricos** son dos formas diferentes de proteger la información, no compiten sino que se complementan
- **Los simétricos** destacan por su velocidad y eficiencia, pero requieren compartir la misma clave
- **Los asimétricos** ofrecen más seguridad, ya que usan dos claves distintas, aunque son más lentos

Gracias al uso de **scripts automatizados**, podemos simplificar y asegurar el proceso de cifrado y descifrado, aplicando estos métodos en entornos reales como empresas o redes seguras

En entornos reales (empresas, redes, servidores), ambos se usan juntos para garantizar confidencialidad, integridad y autenticidad

## Comparativa Final

| **Característica** | **Simétrico**                                        | **Asimétrico**                                         |
| --------------     | ---------------------------------------------------- | ------------------------------------------------------ |
| **Claves**         | 1 clave compartida                                   | Clave pública + clave privada                          |
| **Velocidad**      | Rápido                                               | Más lento                                              |
| **Seguridad**      | Depende de ocultar la clave                          | Alta seguridad, claves separadas                       |
| **Uso ideal**      | Cifrado de archivos, VPN, grandes volúmenes de datos | Intercambio de claves, autenticación, firmas digitales |
