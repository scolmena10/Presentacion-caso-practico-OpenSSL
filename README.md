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

Creamos el directorio cifrado_simetrico en Ubuntu para agrupar las claves, mensajes y scripts

```
mkdir cifrado_simetrico
cd cifrado_simetrico
```
Creamos un archivo de texto llamado mensaje.txt con el comando 
```
echo "(mensaje para cifrar)" > mensaje.txt
```
y después comprobamos si se ha creado con el comando ( cat mensaje.txt )
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
> [!NOTE]
> #### ¿Qué hace este script?
>
> - Genera una clave segura de 256 bits
> - Cifra el mensaje usando AES-256-CBC
> - Guarda la clave en clave_sim.hex

Creamos el fichero con el comando
```
sudo nano desencriptado.sh
```
Y agregaremos el script que desencriptará el mensaje

```bash
#!/bin/bash

openssl enc -d -aes-256-cbc -pbkdf2 -in mensaje.enc -out mensaje_decrypted.txt -pass file:./clave_sim.hex

echo "Descifrado completado: se generó mensaje_decrypted.txt"
```

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
> 
> - Santino usa la clave pública de la empresa para cifrar un mensaje, y solo la empresa con su clave privada puede descifrarlo



