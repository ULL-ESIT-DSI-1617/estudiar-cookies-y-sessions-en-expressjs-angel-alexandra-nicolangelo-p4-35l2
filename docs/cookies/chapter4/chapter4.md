# Set-Cookie

El encabezado de respuesta HTTP Set-Cookies HTTP es usado para enviar cookies 
desde el servidor al usuario agente .

## Sintaxis

    Set-Cookie: <cookie-name>=<cookie-value> 
    Set-Cookie: <cookie-name>=<cookie-value>; Expires=<date>
    Set-Cookie: <cookie-name>=<cookie-value>; Max-Age=<non-zero-digit>
    Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>
    Set-Cookie: <cookie-name>=<cookie-value>; Path=<path-value>
    Set-Cookie: <cookie-name>=<cookie-value>; Secure
    Set-Cookie: <cookie-name>=<cookie-value>; HttpOnly

    Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Strict
    Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Lax

    // Multiple directives are also possible, for example:
    Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; HttpOnly
    
## Directivas

    <cookie-name>=<cookie-value>
    
    Una cookie empieza con un name-value par:
        * <cookie-name> 
        * <cookie-value>
        * __Secure-prefix: 
        * __Host-prefix:
    
    `$ Expires=<date> `
    `$ Max-Age=<non-zero-digit> `
    `$ Domain=<domain-value> `
    `$ Path=<path-value> `
    `$ Secure`
    `$ HttpOnly `
    `$ SameSite=Strict ` 
    `$ SameSite=Lax `
    
## Ejemplos

#### Cookie de sesión
La cookie de sesión se eliminará cuando el cliente apague la máquina.

`$ Set-Cookie: sessionid=38afes7a8; httponly; Path=/ `

#### Cookie permanente
En lugar de expirar cuando el cliente cierra sesión, 
la cookie permanente expira a una fecha específica.

`$ Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly `

#### Third party cookie (Cookie de terceros)
La cookie de terceros pertenece a un dominio diferente al actualmente visto en la barra de direcciones.
Esta cookie se establece normalmente por anuncios y abren el potencial para el seguimiento del historial de navegación del usuario.

`$ Set-Cookie: qwerty=219ffwef9w0f; Domain=somecompany.co.uk; Path=/; Expires=Wed, 30 Aug 2019 00:00:00 GMT `

#### Prefijos de cookies
Los nombres de las cookies con prefijos "__Secure-" y "__Host-" se usan solo si están establecidas
con la directiva segura desde un origen seguro (HTTPS).

    // Both accepted when from a secure origin (HTTPS)
    Set-Cookie: __Secure-ID=123; Secure; Domain=example.com
    Set-Cookie: __Host-ID=123; Secure; Path=/

    // Rejected due to missing Secure directive
    Set-Cookie: __Secure-id=1

    // Rejected due to the missing Path=/ directive (unless at root of the site)
    Set-Cookie: __Host-id=1; Secure

    // Rejected due to setting a domain
    Set-Cookie: __Host-id=1; Secure; Path=/; domain=example.com
    
    
#### Compatibilidad en el navegador

Escritorio

|Feature 	    |Chrome 	|Edge 	    |Firefox 	|Internet Explorer| Opera 	|Safari 	|Servo      |
|---------------|:----------|:----------|:----------|:----------------|:--------|:----------|:----------|
|Basic Support	|(Yes)	    |(Yes)	    |(Yes)	    |(Yes)	          | (Yes)	|   (Yes)	|   ?       |       
|Max-Age	    |(Yes)	    |(Yes)	    |(Yes)	    | 8.0	          | (Yes)	|   (Yes)	|   ?       |       
|HttpOnly	    | 1.0	    |(Yes)	    | 3.0	    | 9.0	          |  11	    |    5.0	|   ?       |       
|Cookie         |           |           |           |                 |         |           |           |       
|prefixes	    | 49	    |(Yes)	    | 50	    | ?	              |  36	    |   (Yes)	|   ?       |       
|SameSite	    | 51	    |No support	|No support1|No support	      |  39	    | No support|	?       |      

