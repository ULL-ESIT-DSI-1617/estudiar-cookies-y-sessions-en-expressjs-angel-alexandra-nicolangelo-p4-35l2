# Chapter 1: GitBook

GitBook fue creado a mediados de 2014 con la visión de crear una solución moderna y sencilla a la documentación, la escritura digital y la publicación. Se ha construido un formato de código abierto. La filosofía es ser simple hasta el punto de la elegancia, eliminando las distracciones y las preocupaciones de los creadores de contenido, para que puedan escribir libremente.

### Instalación local

Necesitamos tener instalado en nuestra máquina:

* NodeJS \(v4.0.0 recomendado\)
* Windows, Linux, Unix o Mac OS X

GitBook tiene una versión escritorio la cual es muy amigable, sencilla y tiene todas las utilidades que puede tener la versión web o por línea de comandos.

### Instalación con NPM

Si queremos utilizar GitBook mediante consola, debemos tener instalado NPM y ejecutar el siguiente comando:

`$ npm install gitbook-cli -g`

Con `gitbook-cli` nos permite utilizar varias versiones de GitBook en el mismo sitema. Este instala automaticamente la version requerida de GitBook para crear un libro.

### Crear un libro

GitBook puede configurar un libro utilizando:

`$ gitbook init`

Para crear el libro en un nuevo directorio utilizamos el comando:

`$ gitbook init ./directory`

Para previsualizar nuestro libro utilizamos:

`$ gitbook serve`

O crear un website estático utilizamos:

`$ gitbook build`

### Desplegar un libro en Gitbook

Hay varias maneras de desplegar un Gitbook.

La primera es la sincronización del libro en Gitbook con el repositorio que queramos en Github, para sincronizarlo vamos al apartado de `settings` en el libro, y nos movemos a la opción de `Github`, ponemos el nombre de nuestro repositorio en Github y pulsamos el botón verde con la palabra `Sync`. Y ya se habría sincronizado.

La segunda es realizar el libro de manera online y que la plataforma se encargue de hacer los `commits` y el `push` cada vez que actualices algo, por lo que Gitbook ya se encargaría de desplegar el libro

### Instalar versiones previas

`gitbook-cli` hace fácil descargar e instalar otras versiones de GitBook para testear nuestro libro:

`$ gitbook fetch beta`

Usamos `gitbook ls-remote` para listar versiones remotas disponibles para instalar.

### Depuración

Podemos usar la opción `--log=debug` y `--debug` para obtener mejores mensajes de error. Por ejemplo:

`$ gitbook build ./ --log=debug --debug`

