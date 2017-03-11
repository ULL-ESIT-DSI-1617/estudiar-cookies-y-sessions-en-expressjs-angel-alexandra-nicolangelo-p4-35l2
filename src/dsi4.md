#Routing API Documentation

El objeto enrutador es una instancia aislada de middleware y rutas. Es una "miniapp" que es capaz de realizar funciones de middleware y de enrutamiento. Todas las aplicaciones de express tiene una app enrutador implícita.

Una vez que has creado un objeto enrutador puedes añadirle rutas de métodos middleware y HTTP (GET,PUT,etc) justo como una aplicación.

Este es un ejemplo de uso:

// invoked for any requests passed to this router

router.use(function(req, res, next) {

// .. some logic here .. like any other middleware

next();

});

// will handle any request that ends in /events

// depends on where the router is "use()'d"

router.get('/events', function(req, res, next) {

// ..

});

Puedes usar un enrutador para pasar una URL raíz de forma que separa tus rutas dentro de archivos o "miniapps"

// only requests to /calendar/* will be sent to our "router"

app.use('/calendar', router);

##Methods

###router.all(path, [callback, ...] callback)
Este método se encarga de todas las solicitudes HTTP. Es muy útil para poner código que tenga que ejecutarse de forma global. Por ejemplo, si el siguiente código lo escribimos en lo más alto de la jerarquía de las definiciones de rutas, significaría que cada vez que accedamos a cualquier directorio, se requerirá de una autenticación.

router.all('*', requireAuthentication, loadUser);

De esta forma, al acceder a cualquier directorio, ejecutaremos la callback requireAuthentication, seguido de loadUser y luego puede haber un next() para la siguiente middleware.



###router.METHOD(path, [callback, ...] callback)

Este método proporciona la funcionalidad de enrutamiento en express para solicitudes HTTP concretas (GET,PUT,etc).
Se pueden especificar varias callbacks, e incluso alguna puede incluir next('route') para saltarse la invocación de una callback determinada en caso de que no sea necesario proceder con la ruta actual.

El siguiente ejemplo ilustra la definición más simple de una ruta.

router.get('/', function(req, res){

res.send('hello world');

});

Usando expresiones regulares:

router.get(/^\/commits\/(\w+)(?:\.\.(\w+))?$/, function(req, res){

var from = req.params[0];

var to = req.params[1] || 'HEAD';

res.send('commit range ' + from + '..' + to);

});

###router.param(name, callback)

Añade "disparadores" a los parámetros de la ruta, donde "name" es el nombre del parámetro y callback es una función callback.

Los parámetros de la función callback son:

req, el objecto solicitado.
res, el objeto que responde.
next, indica la siguiente middleware.
El valor del parámetro name.
El nombre del parámetro.
Por ejemplo, cuando :user está presente en el camino de una ruta, puedes ejecutar código para que automáticamente se facilite req.user a la ruta.

router.param('user', function(req, res, next, id) {

// try to get the user details from the User model and attach it to the request object

User.find(id, function(err, user) {

if (err) {

next(err);

} else if (user) {

req.user = user;

next();

} else {

next(new Error('failed to load user'));

}

});

});

Las funciones callback de param son locales al router en el que son definidas. No se heredan en apps o router subsiguientes. Por tanto, las callbakc de param definidas en router serán ejecutadas solo por parámetros de ruta definidos en las rutas de router.

Una callback de param será llamada solo una vez en un ciclo solicitud-respuesta, incluso si el parámetro coincide en varias rutas, como se muestra a continuación:

router.param('id', function (req, res, next, id) {

console.log('CALLED ONLY ONCE');

next();

});

router.get('/user/:id', function (req, res, next) {

console.log('although this matches');

next();

});

router.get('/user/:id', function (req, res) {

console.log('and this matches too');

res.end();

});

Si ejecutamos GET /user/42, se imprime lo siguiente en la consola:

CALLED ONLY ONCE

although this matches

and this matches too


###router.route(path)

Devuelve una instancia de una sola ruta la cual se puede usar para manejar peticiones HTTP con middleware opcionales. Usa router.route() para evitar nombres de ruta duplicados y errores de escritura.

Partiendo del ejemplo de router.param() de arriba, el siguiente código muestra como usar router.route() para especificar varios controladores de métodos HTTP.

var router = express.Router();

router.param('user_id', function(req, res, next, id) {

// sample user, would actually fetch from DB, etc...

req.user = {

id: id,

name: 'TJ'

};

next();

});

router.route('/users/:user_id')

.all(function(req, res, next) {

// runs for all HTTP verbs first

// think of it as route specific middleware!

next();

})

.get(function(req, res, next) {

res.json(req.user);

})

.put(function(req, res, next) {

// just an example of maybe updating the user

req.user.name = req.params.name;

// save user ... etc

res.json(req.user);

})

.post(function(req, res, next) {

next(new Error('not implemented'));

})

.delete(function(req, res, next) {

next(new Error('not implemented'));

});


###router.use([path], [function, ...] function)

Usa el middleware o middleware especificado, con un punto de montaje path opciones, que por defecto es /.

Este método es similar al app.use().

var express = require('express');

var app = express();

var router = express.Router();

// simple logger for this router's requests

// all requests to this router will first hit this middleware

router.use(function(req, res, next) {

console.log('%s %s %s', req.method, req.url, req.path);

next();

});

// this will only be invoked if the path starts with /bar from the mount point

router.use('/bar', function(req, res, next) {

// ... maybe some additional /bar logging ...

next();

});

// always invoked

router.use(function(req, res, next) {

res.send('Hello World');

});

app.use('/foo', router);

app.listen(3000);

El orden en que defines los middlewares con router.use() es muy importante ya que se invocan de forma secuencial, de arriba a abajo. Por eso Lo normal es poner como primer middleware uno de loging.

var logger = require('morgan');

router.use(logger());

router.use(express.static(__dirname + '/public'));

router.use(function(req, res){

res.send('Hello');

});