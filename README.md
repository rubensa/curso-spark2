# Curso de Spark2 con VSCode

# Ubicación del software
El software de instalación se encuentra disponible en el Almacén de Software de la Unidad de Operaciones ( smb://almacen/operaciones/ ), en la carpeta **/software/programming/javascript/vscode**.

Según la versión del Sistema Operativo que tengamos procederemos a instalar la versión de Visual Studio Code correspondiente:

  * **code-stable-1573664143.tar.gz** (Linux 64 bits)
  * **VSCode-win32-ia32-1.40.1.zip** (Windows 32 bits)
  * **VSCode-win32-x64-1.40.1.zip** (Windows 64 bits)
  * **VSCode-darwin-stable.zip** (Mac)

**NOTA**

En Linux será necesario instalar el paquete **libgconf-2-4** (error while loading shared libraries: libgconf-2.so.4: cannot open shared object file: No such file or directory) si éste no se encuentra previamente instalado.

La instalación consiste en descomprimir el fichero correspondiente en la ubicación deseada (por ejemplo **~/software/vscode**).

# Preparación del entorno de desarrollo
Creamos una carpeta que albergará la información necesaria para la ejecución de nuestro entorno de desarrollo:

  * **~/work/vscode**

Dentro de ella creamos la carpeta **extensions**. Dicha carpeta contendrá las extensiones que utilizaremos para el desarrollo de nuestros proyectos (cada proyecto concreto podría necesitar diferentes extensiones). Si ya tenemos alguna extensión descargada previamente, la podemos guardar en dicha carpeta.

Del mismo modo creamos la carpeta **config**. Dicha carpeta albergará la configuración a aplicar al entorno de desarrollo de VSCode que arranquemos.

Por ejemplo, podemos deshabilitar el envío de datos de utilización y las actualizaciones automáticas así como otra serie de preferencias, guardando el fichero **config/User/settings.json** con el siguiente contenido:

```json
{
    // Enable usage data and errors to be sent to Microsoft.
    "telemetry.enableTelemetry": false,
    // Automatically update extensions
    "extensions.autoUpdate": false,
    // Configure whether you receive automatic updates from an update channel.
    "update.channel": "none",
    // Disable preview mode
    "workbench.editor.enablePreview": false
}
```

La estructura creada será similar a la siguiente:
```
~/work/vscode
~/work/vscode/extensions
~/work/vscode/config
~/work/vscode/config/User
~/work/vscode/config/User/settings.json
```

# Ejecución del entorno de desarrollo
Para arrancar el entorno de desarrollo crearemos un lanzador (por ejemplo **~/.local/bin/code**) que ejecute el VSCode pasándole como parámetro, en la línea de comandos, la configuración adecuada.

```bash
#!/usr/bin/env bash
~/software/vscode/code --user-data-dir "~/work/vscode/config" --extensions-dir "~/work/vscode/extensions" "$@"
```

**NOTA**  Una vez creado hay que darle permisos de ejecución.

Del mismo modo que hemos creado un lanzador de línea de comando, podemos hacer lo mismo para el escritorio (por ejemplo **~/.local/share/applications/vscode.desktop**)

```
[Desktop Entry]
Name=Visual Studio Code
Comment=Code Editing. Redefined.
GenericName=Text Editor
Exec=code %F
Icon=vscode
Type=Application
StartupNotify=false
StartupWMClass=code
Categories=Utility;TextEditor;Development;IDE;
MimeType=text/plain;inode/directory;
Keywords=vscode;
```

# Instalación de extensiones
Una vez instalado podemos proceder a arrancar el entorno (usando el comando **code**) e instalar las extensiones que requiera nuestro proyecto que, como ya hemos comentado, se guardarán en la carpeta **extensions** de nuestro directorio de configuración. De este modo, podremos reutilizar una misma instalación del VSCode con diferentes extensiones y configuraciones.  En este caso, debemos instalar las siguientes extensiones:
  * [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) (ms-vscode-remote.vscode-remote-extensionpack)

# Descarga del proyecto
Descargamos el proyecto de https://github.com/rubensa/curso-spark2 usando git:

```bash
cd ~/work
git clone https://github.com/rubensa/curso-spark2.git
```

# Configuración del proyecto
En el fichero **.devcontainer/Dockerfile** debemos especificar nuestro usuario y grupo (para que la imagen Docker creada utilice dicha información):

```Dockerfile
# $(id -un)
ARG NEW_USER_NAME=rubensa
# $(id -gn)
ARG NEW_GROUP_NAME=rubensa
```

Del mimso modo, en el fichero .devcontainer/devcontainer.json debemos especificar nuestro ID de usuario e ID de grupo por defecto:

```json
        // $(id -u):$(id -g)
        "-u", "1000:1000"
```

**NOTA**:  Ejecutando los comandos que aparecen en los comentarios podemos obtener los valores que debemos utilizar

# Arranque del entorno de desarrollo
No situamos en el directorio donde hemos descargado el poyecto (por ejemplo ~/work/curso-spark2) y arrancamos desde dicho directorio el VSCode:

```bash
code ~/work/curso-spark2
```

El plugin de desarrollo remoto reconocerá que la carpeta contiene configuración para el desarrollo en un contenedor y nos preguntará si deseamos volver a abrirlo en un contenedor, a lo que responderemos afirmativamente.

La condiguración de la imagen Docker tardará un rato.  A partir de este momento, estaremos trabajando dentro del contenedor Docker (si abrimos una terminal dentro del VSCode, dicha terminal se ejecutará dentro del contenedor).