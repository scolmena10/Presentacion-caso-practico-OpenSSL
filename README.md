# Presentación caso práctico OpenSSL
## Cifrado Simétrico vs Asimétrico y Automatización con Scripts

> [!NOTE]
> ## Objetivo
> Nuestro proyecto tiene como objetivo comparar los algoritmos de cifrado simétrico y asimétrico, entender sus diferencias y ventaja. Lo haremos en su mayoria con la ayuda de scripts en Bash usando OpenSSL.

> [!IMPORTANT]
> ## Integrantes
> - **Abdullah** – Conceptos y fundamentos del cifrado simétrico y asimétrico.
> - **David** – Implementación práctica del cifrado simétrico AES-256-CBC mediante scripts.
> - **Carlos** – Explicación teórica del cifrado asimétrico, claves y aplicaciones.
> - **Santino** – Implementación práctica del cifrado asimétrico con RSA mediante scripts.

---

# Introducción a la Criptografía
## Cifrado Simétrico
El Cifrado Simétrico es tipo de algoritmos criptográfico que usa la misma clave para cifrar como para descifrar información (La misma clave secreta que se usa para convertir el texto original que ha sido cifrado también se usa para volver a obtener el texto original)

### Características

- **Una única clave:** Se une para cifrar y descifrar la misma clave, lo positivo y negativo de esto es que, si tu y tu amigo compartes la misma clave, ambos podéis cifrar y descifrar los mensajes, pero si alguien adivina vuestra clave, podrá ver todos los mensajes
- **Rapidez y eficiencia:** Los algoritmos simétricos son rápidos y eficientes, por eso lo usan para transmisión de datos, almacenamiento cifrado y VPNs
- **Seguridad:** El cifrado no depende del algoritmo en sí, sino de mantener la clave secreta, si la clave es segura (larga, aleatoria, con símbolos…) El cifrado será muy complicado de romper, en cambio si es una palabra común será muy sencilla de averiguar
  
#### Problema Principal
El gran problema de esto es que, ambas personas necesitan tener la misma clave secreta para poder comunicarse y antes de usarla uno se la tiene que enviar al otro, ahí está el problema. Si lo enviamos por WhatsApp, correo o redes, puede ser interceptada

#### Solución
Se puede usar otro tipo de cifrado llamado asimétrico, que no necesita compartir la misma clave, por ejemplo:
Yo tengo una clave pública que la puede saber todo el mundo para cifrar los mensajes y una privada que sólo se yo, cualquiera puede usar mi clave pública para poder cifrar el mensaje y solo yo con la privada descifrarlo.

---

## Cifrado Asimétrico
Los algoritmos asimétricos son un tipo de criptografía que usa dos claves diferentes relacionadas matemáticamente y usan dos claves una pública y otra privada.
- **Clave pública:** se comparte libremente.
- **Clave privada:** debe mantenerse protegida.
