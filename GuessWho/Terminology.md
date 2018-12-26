# Terminología
Este artículo proporciona definiciones para los términos que son específicos para el sistema.

## Contexto
Supongamos que la Compañía XYZ desea facilitar a sus clientes un código de activación para ser utilizado en una campaña telefónica de suscripción a un boletín de noticias.  

En otras palabras, el cliente marcará a la central telefónica de la Compañía XYZ, donde se le pedirá que ingrese su número de identificación y su código de activación. 

## ¿Cómo llega al cliente su código de activación?
### Enviado por la Compañía XYZ

Hay varias formas. La más sencilla es que la Compañía XYZ a través de algún proceso automático se lo envió como un SMS a su número de contacto,  o se lo envió a su correo electrónico. 

Otra forma es que la Compañía XYZ le muestra al cliente el código cuando este hace uso de alguna de sus plataformas, como por ejemplo su página web. 

En cualquiera de los escenarios anteriores, la Compañía XYZ es la encargada de generar el código y enviárselo a su cliente. Para estos escenarios, la Compañía XYZ nos debe entregar un archivo con el formato GW-01 (descrito más adelante), para que nosotros carguemos esta información en nuestro sistema. Este archivo es simplemente una relación del tipo: Para el usuario con número de identificación 111111111 el código de activación es  999999.

### Enviado por Evertec
Si el archivo contiene la información del número de contacto (celular) o correo electrónico de los clientes, nuestro sistema está en capacidad de enviar el SMS o correo. Si el archivo NO contiene un código de activación, nuestro sistema generará uno aleatoriamente, lo asociará con el número de identificación y lo guardara de forma segura (cifrado). 
En otras palabras, nuestro cliente nos debe entregar un archivo con la relación de los números de identificación que desea enrolar en la campaña, adicionando el número de contacto (celular) o el correo electrónico. 




## Definiciones de términos

- **Código de activación**: Contraseña de un solo uso (compuesta por varios digitos, <_generalmente seis_>) que se puede utilizar para `autenticar` a un usuario. Funciona como un [OTP](https://en.wikipedia.org/wiki/One-time_password), asociado con el identificador de un usuario en una institución. Por ejemplo _999999_
- **Institución**: Representa una compañía, emisor o cliente en el sistema. Por ejemplo: _Compañia XYZ_
- **Campaña**: Una campaña está asociada con una institución y representa el proposito para el que se emitieron un grupo de códigos de activación. Por ejemplo: _Suscripción al boletín de noticias_.
- **Canal**: Representa el medio o recurso desde el que  se puede utilizar un código de activación. Por ejemplo: _SMS, Telegram, Facebook, IVR, CallCenter_, etc.
- **Política**: Representa las validaciones que se realizan cuando un usuario intenta utilizar un código de activación. Por ejemplo: _Máximo 3 intentos invalidos_ o _Máximo 1 intento invalido_ o _permita todos los intentos que desee_
<a name="guestbook"></a>
- **Libro de invitaciones**: Almacena la información de las _personas/clientes_ de una institución que han sido invitados a participar de una campaña.
- **Libro de rechazos**: Almacena la información de los intentos de utilización de códigos de activación que han sido invalidos y su motivo, es decir, guarda la información de la(s) política(s) por la que no se pudo validar un código de activación.
- **Libro de firmas**: Almacena la información de los códigos de validación que se han dado de alta correctamente.




### Información relacionada
- [Archivo formato GW-01](File-Format.md)
