# Pandoc
Si necesitas pasar un archivo de un formato a otro, Pandoc es la herramienta que necesitas.
Pandoc puede convertir un documento en MarkDown, HTML, DocBook, Microsoft Word, Libre Office, etc a cualquier otro formato.

![Imagen](https://camo.githubusercontent.com/899079dfc5d673045c039406fe84ae42c3e331a9/687474703a2f2f66696c65732e737761726f6f7063682e636f6d2f6b616c616d2f53637265656e2d53686f742d323031322d31322d30342d61742d312e35312e31362d504d2e706e67 "Pandoc on Mac")

## Ejemplo de comandos
* **Un docuemento de libreoffice:**

    ` $ pandoc ejemplo.md ejemplo.odt`

* **Un fichero html completo con cabeceras:**

    ` $ pandoc -s ejemplo.md ejemplo.html`

* **Un fichero tex:**

    ` $ pandoc ejemplo.md -o ejemplo.tex`

* **Desde un fichero html:**

    ` $ pandoc ejemplo.html -o ejemplo.pdf `
    
* **Desde un fichero tex:**

    ` $ pandoc ejemplo.tex -o ejemplo.pdf`

![Imagen2](https://i2.wp.com/blog.desdelinux.net/wp-content/uploads/2012/10/diagram.png)

## Instalar Pandoc en Windows

* Este es el instalador de paquetes de la [página de pándoc](https://github.com/jgm/pandoc/releases/tag/1.19.2.1)
* Para la salida de PDF, también necesitarás instalar LaTeX. Te recomendamos MiKTeX.
* Si lo prefieres, puedes extraer los archivos ejecutables Pandoc y pandoc-citeproc del MSI y copiarlos diréctamente a cualquier directorio, sin ejecutar el instalador. 
A continuación se muestra un ejemplo de cómo extraer los ejecutables del instalador 
pandoc-1.19.1 y copiarlos en: `$ C:\Utils\Console`

>   mkdir "%TEMP%\pandoc\"
    start /wait msiexec.exe /a pandoc-1.19.1-windows.msi /qn targetdir="%TEMP%\pandoc\"
    copy /y "%TEMP%\pandoc\pandoc.exe" C:\Utils\Console\
    copy /y "%TEMP%\pandoc\pandoc-citeproc.exe" C:\Utils\Console\
    rmdir /s /q "%TEMP%\pandoc\"