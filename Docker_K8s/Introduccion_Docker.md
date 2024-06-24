# ¿Qué es es Docker?

<p align=center>
<img src="images/docker/docker.webp" width="200" >
</p>

Docker es una plataforma abierta para desarrollar, enviar y ejecutar aplicaciones. Docker brinda la capacidad de empaquetar y ejecutar una aplicación en un entorno poco aislado llamado contenedor. El aislamiento y la seguridad le permiten ejecutar muchos contenedores simultáneamente en un host determinado. Los contenedores son livianos y contienen todo lo necesario para ejecutar la aplicación, por lo que no es necesario depender de lo que está instalado en el host.

## Alcance de este curso:

- Introducción a contenedores
- Arquitectura
- Componentes
- Instalación
- Comandos basicos
- Ejecución de imagenes
- Construcción de imagenes
- Volúmenes
- Multi-Contenedores
  
# Introducción de contenedores

Los contenedores son unidades ejecutables de software que empaquetan el código de la aplicación junto con sus bibliotecas y dependencias. Permiten que el código se ejecute en cualquier entorno informático, ya sea de escritorio, de TI tradicional o de infraestructura en la nube. Los contenedores aprovechan una forma de virtualización del sistema operativo (SO) en la que las características del kernel del sistema operativo (por ejemplo, espacios de nombres y cgroups de Linux, silos de Windows y objetos de trabajo) se pueden utilizar para aislar procesos y controlar la cantidad de CPU, memoria y disco al que esos procesos pueden acceder.

- Entrega rápida y consistente de sus aplicaciones

    - Docker agiliza el ciclo de vida de desarrollo al permitir que los desarrolladores trabajen en entornos estandarizados utilizando contenedores locales que brindan sus aplicaciones y servicios. Los contenedores son excelentes para flujos de trabajo de integración y entrega continua (CI/CD).

- Implementación y escalamiento responsivos

    - La plataforma basada en contenedores de Docker permite cargas de trabajo altamente portátiles. Los contenedores Docker pueden ejecutarse en la computadora portátil local de un desarrollador, en máquinas físicas o virtuales en un centro de datos, en proveedores de nube o en una combinación de entornos.

- Ejecutar más cargas de trabajo en el mismo hardware

    - Docker es ligero y rápido. Proporciona una alternativa viable y rentable a las máquinas virtuales basadas en hipervisor, para que pueda utilizar una mayor capacidad de su servidor para lograr sus objetivos comerciales. Docker es perfecto para entornos de alta densidad y para implementaciones pequeñas y medianas donde necesitas hacer más con menos recursos.
 
#### Contenedores

Un contenedor es una instancia ejecutable de una imagen. Puede crear, iniciar, detener, mover o eliminar un contenedor mediante la API o CLI de Docker. Puedes conectar un
contenedor a una o más redes, adjuntarle almacenamiento o incluso crear una nueva imagen según su estado actual.

De forma predeterminada, un contenedor está relativamente bien aislado de otros contenedores y su máquina anfitriona. Puede controlar el grado de aislamiento de la red, el almacenamiento y u otros subsistemas subyacentes son de otros contenedores o del host máquina.

Un contenedor se define por su imagen, así como por las opciones de configuración que proporcionarle cuando lo cree o inicie. Cuando se retira un contenedor, cualquier cambio en
su estado que no está almacenado en un almacenamiento persistente desaparece.

#### Imágenes

Una imagen es una plantilla de solo lectura con instrucciones para crear un Docker envase. A menudo, una imagen se basa en otra imagen, con algunos detalles adicionales personalizados. Por ejemplo, puede crear una imagen basada en `ubuntu`, pero instala el servidor web Apache y su aplicación, así como el detalles de configuración necesarios para que su aplicación se ejecute.

Puedes crear tus propias imágenes o usar solo aquellas creadas por otros y publicadas en un registro. Para construir su propia imagen, crea un Dockerfile con una sintaxis simple para definir los pasos necesarios para crear la imagen y ejecutarla. Cada instrucción en un Dockerfile crea una capa en la imagen. Cuando usted cambie el Dockerfile y reconstruya la imagen, solo aquellas capas que tengan modificados se reconstruyen. Esto es parte de lo que hace que las imágenes sean tan ligeras, pequeñas y y rápido, en comparación con otras tecnologías de virtualización.

### Alternativas

- Docker: Es una plataforma abierta para desarrollar, enviar y ejecutar aplicaciones. Docker le permite separar sus aplicaciones de su infraestructura para que pueda entregar software rápidamente. Con Docker, puede administrar su infraestructura de la misma manera que administra sus aplicaciones. Al aprovechar las metodologías de Docker para enviar, probar e implementar código, puede reducir significativamente el retraso entre escribir el código y ejecutarlo en producción.

- Jenkins: Es un servidor de automatización de código abierto autónomo que se puede utilizar para automatizar todo tipo de tareas relacionadas con la creación, prueba y entrega o implementación de software.

- Podman: Es una herramienta nativa de Linux, de código abierto y sin demonio, diseñada para facilitar la búsqueda, ejecución, creación, uso compartido e implementación de aplicaciones utilizando contenedores e imágenes de contenedores de la Open Containers Initiative ( OCI ).

- Apptainer/Singularity: Es una plataforma de contenedores creada para el caso de uso HPC/HTC. Permite construir y ejecutar contenedores con sólo unos pocos pasos en la mayoría de los casos, y su diseño presenta ventajas clave para la comunidad científica:

    - Imágenes de contenedores basadas en un solo archivo, que facilitan la distribución, el archivado y el intercambio.
    - Capacidad de ejecutarse, y en sistemas modernos también de instalarse, sin ningún demonio raíz o privilegios setuid.
    - Conserva los permisos en el entorno. El usuario fuera del contenedor puede ser el mismo usuario dentro.

- Sarus: Es un software para ejecutar contenedores Linux en entornos de Computación de Alto Rendimiento. Su desarrollo ha sido impulsado por los requisitos específicos de los sistemas HPC, al tiempo que aprovecha estándares y tecnologías abiertos para fomentar la participación de los proveedores y la comunidad.

## Arquitectura

Docker utiliza una arquitectura cliente-servidor. El cliente Docker se comunica con el demonio Docker, que realiza el trabajo pesado de crear, ejecutar y distribuir sus contenedores Docker. El cliente Docker y el demonio pueden ejecutarse en el mismo sistema, o puede conectar un cliente Docker a un demonio Docker remoto. El cliente Docker y el demonio se comunican mediante una API REST, a través de sockets UNIX o una interfaz de red. Otro cliente Docker es Docker Compose, que le permite trabajar con aplicaciones que constan de un conjunto de contenedores.

<p align=center>
<img src="images/docker/docker-architecture.webp" width="1000" >
</p>

### El demonio Docker

El demonio Docker (`dockerd`) escucha las solicitudes de la API de Docker y administra Docker objetos como imágenes, contenedores, redes y volúmenes. Un demonio también puede comunicarse con otros demonios para administrar los servicios de Docker.

### El cliente Docker

El cliente Docker (`docker`) es la forma principal en que interactúan muchos usuarios de Docker con Docker. Cuando usa comandos como `docker run`, el cliente envía estos
comandos a `dockerd`, que los ejecuta. El comando `docker` usa el API de Docker. El cliente Docker puede comunicarse con más de un demonio.

# Instalación de Docker *

Para instalar Docker en nuestro dispositivo hay distintas opciones:

- Windows
    - [Instalar WSL (Windows Subsystem for Linux)](https://learn.microsoft.com/es-es/windows/wsl/install)
    - [Instalar Docker Hub](https://hub.docker.com/open-desktop)
- Linux
    - Command Line Interface (CLI)   

## Comandos para instalar Docker en Linux

    $ sudo apt update

    $ sudo apt install apt-transport-https ca-certificates curl software-properties-common 

    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg 

    $ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 

    $ sudo apt update

    $ apt-cache policy docker-ce 

    $ sudo apt install docker-ce 

## Comprobamos que Docker este funcionando

    $ sudo systemctl status docker 


```console
[sudo] password for user:
● docker.service - Docker Application Container Engine
    Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
    Active: active (running) since Thu 2024-06-20 10:55:07 CST; 20min ago
TriggeredBy: ● docker.socket
    Docs: https://docs.docker.com
Main PID: 260 (dockerd)
    Tasks: 17
    Memory: 118.0M
    CGroup: /system.slice/docker.service
            └─260 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

    
#### Opcional eliminar sudo en docker 

1.
    ```console
    $ sudo usermod -aG docker ${USER} 
    ```
2.
    ```console
    $ su - ${USER} 
    ```

# Comandos basicos de Docker

1. Mostrar información del sistema

    ```console
    $ docker info
    ```

    ```console
    $ Client: Docker Engine - Community
        Version:    26.1.2
        Context:    default
        Debug Mode: false
    ```

2. Mostrar la versión del sistema

    ```console
    $ docker version
    ```

    ```console
    $  Client: Docker Engine - Community
        Version:           26.1.2
        API version:       1.45
        Go version:        go1.21.10
        Git commit:        211e74b
        Built:             Wed May  8 13:59:59 2024
        OS/Arch:           linux/amd64
        Context:           default

        Server: Docker Engine - Community
        Engine:
        Version:          26.1.2
        API version:      1.45 (minimum version 1.24)
        Go version:       go1.21.10
        Git commit:       ef1912d
        Built:            Wed May  8 13:59:59 2024
        OS/Arch:          linux/amd64
        Experimental:     false
        containerd:
        Version:          1.6.31
        GitCommit:        e377cd56a71523140ca6ae87e30244719194a521
        runc:
        Version:          1.1.12
        GitCommit:        v1.1.12-0-g51d5e94
        docker-init:
        Version:          0.19.0
        GitCommit:        de40ad0
    ```

# Prueba Hello World

Descargamos la imagen de prueba para verificar que nuestra instalación de Docker.

-  
    ```console
    $ docker pull hello-world
    $ docker run hello-world
    ```

   ```console
    Hello from Docker!
        This message shows that your installation appears to be working correctly.

        To generate this message, Docker took the following steps:
        1. The Docker client contacted the Docker daemon.
        2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
            (amd64)
        3. The Docker daemon created a new container from that image which runs the
            executable that produces the output you are currently reading.
        4. The Docker daemon streamed that output to the Docker client, which sent it
            to your terminal.

        To try something more ambitious, you can run an Ubuntu container with:
        $ docker run -it ubuntu bash

        Share images, automate workflows, and more with a free Docker ID:
        https://hub.docker.com/

        For more examples and ideas, visit:
        https://docs.docker.com/get-started/

   ```

# Descarga de imagenes

1. Descargamos la imagen Nginx desde Docker Hub

    ```console
    $ docker run -d -p 8080:80 --name webserver nginx
    
    Unable to find image 'nginx:latest' locally
    latest: Pulling from library/nginx
    2cc3ae149d28: Pull complete
    1018f2b8dba8: Pull complete
    b831e78d8e20: Pull complete
    3ab22521e919: Pull complete
    5112bf42775b: Pull complete
    cbdaf9e4ee2d: Pull complete
    a06b6fd631e8: Pull complete
    Digest: sha256:9c367186df9a6b18c6735357b8eb7f407347e84aea09beb184961cb83543d46e
    Status: Downloaded newer image for nginx:latest
    ef5849a2c694b3f36ceeb307b27184006e1d4765c3d18b46b778a8c47edb303f
    ```

2. Listar los contenedores

    ```console
    $ docker ps
    
    CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
    ef5849a2c694   nginx     "/docker-entrypoint.…"   48 seconds ago   Up 46 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   webserver
    ```

3. Listar las imagenes
    
    ```console
    $ docker images

    REPOSITORY                    TAG       IMAGE ID       CREATED       SIZE
    nginx                         latest    e0c9858e10ed   3 days ago    188MB
    gusdanMX/yelpcamp             v1        76f5fa881385   3 weeks ago   169MB
    gcr.io/k8s-minikube/kicbase   v0.0.44   5a6e59a9bdc0   6 weeks ago   1.26GB
    ```

4. Entrar al contenedor por medio *bash*

    ```console
    $ docker container exec -it  webserver bash  
    # exit
    ```

5. Detener el contenedor

    ```console
    $ docker stop webserver
    ```

6. Remover el contenedor de la memoria

    ```console
    $ docker rm webserver
    ```

7. Remover la imagen

    ```console
    $ docker rmi nginx
    ```

# Descarga la app de prueba

Antes de poder correr la aplicación, debes de descargar el codígo fuente.

1. Clona la app desde [getting-started-app repository](https://github.com/docker/getting-started-app/tree/main) usando el siguiente comando:

   ```console
   $ git clone https://github.com/docker/getting-started-app.git
   ```

2. Visualiza el contenido del repositorio, deberas ver una vista similar a esta.

   ```text
   ├── getting-started-app/
   │ ├── package.json
   │ ├── README.md
   │ ├── spec/
   │ ├── src/
   │ └── yarn.lock
   ```

## Construir la imagen de la aplicación

Para crear la imagen, necesitará utilizar un Dockerfile. Un Dockerfile es simplemente un archivo basado en texto sin extensión de archivo que contiene un script de instrucciones. Docker usa este script para crear una imagen de contenedor.

1. En la carpeta `getting-started-app`, en el mismo lugar que el archivo `package.json`, crea un archivo llamado `Dockerfile`. Puede utilizar los siguientes comandos para crear un Dockerfile basado en su sistema operativo.

   En la terminal, ejecuta el siguiente comando.

   Asegurate de estar en el directorio `getting-started-app`.

   ```console
   $ cd /path/to/getting-started-app
   ```

   Crea un archivo vacío llamado `Dockerfile`.

   ```console
   $ touch Dockerfile
   ```

2. Usando un editor de texto o editor de código, agregue el siguiente contenido al Dockerfile:

   ```dockerfile
   # syntax=docker/dockerfile:1
   
   FROM node:18-alpine
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   EXPOSE 3000
   ```

3. Construye la imagen usando los siguientes comandos:

   ```console
   $ cd /path/to/getting-started-app
   ```

4. Construye la imagen.
   ```console
   $ docker build -t getting-started .
   ```

   El comando `docker build` utiliza Dockerfile para crear una nueva imagen. Es posible que hayas notado que Docker descargó muchas "capas". Esto se debe a que le indicó al constructor que deseaba comenzar desde la imagen `nodo:18-alpine`. Pero, como no lo tenía en su máquina, Docker necesitaba descargar la imagen.

   Después de que Docker descargó la imagen, las instrucciones del Dockerfile se copiaron en su aplicación y usaron `yarn` para instalar las dependencias de su aplicación. La directiva `CMD` especifica el comando predeterminado que se ejecutará al iniciar un contenedor desde esta imagen.

   Finalmente, la bandera `-t` etiqueta tu imagen. Piense en esto como un nombre legible por humanos para la imagen final. Dado que nombró la imagen "introducción", puede consultar esa imagen cuando ejecute un contenedor.

   El `.` al final del comando `docker build` le dice a Docker que debe buscar el `Dockerfile` en el directorio actual.

## Iniciar la aplicación en el contenedor

Ahora que tiene una imagen, puede ejecutar la aplicación en un contenedor usando el comando `docker run`.

1. Ejecute su contenedor usando el comando `docker run` y especifique el nombre de la imagen que acaba de crear:

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 getting-started
   ```

    La bandera `-d` (abreviatura de `--detach`) ejecuta el contenedor en segundo plano.
    Esto significa que Docker inicia su contenedor y lo regresa a la terminal.
    inmediato. Puede verificar que un contenedor se está ejecutando visualizándolo en Docker
    Panel de control en **Contenedores**, o ejecutando `docker ps` en la terminal.

    El indicador `-p` (abreviatura de `--publish`) crea una asignación de puertos entre el host
    y el contenedor. El indicador `-p` toma un valor de cadena en el formato de
    `HOST:CONTENEDOR`, donde `HOST` es la dirección del host y `CONTENEDOR` es
    el puerto en el contenedor. El comando publica el puerto 3000 del contenedor en
    `127.0.0.1:3000` (`localhost:3000`) en el host. Sin el mapeo de puertos,
    no podrá acceder a la aplicación desde el host.

2. Despues de unos segundos, abre tu navegador y navega a la siguiente dirección [http://localhost:3000](http://localhost:3000).


Ejecute el siguiente comando `docker ps` en una terminal para enumerar sus contenedores.

```console
$ docker ps
```
Debería aparecer un resultado similar al siguiente.
```console
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
df784548666d        getting-started     "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        127.0.0.1:3000->3000/tcp   priceless_mcclintock
```

# Sistema de archivos del contenedor

Cuando se ejecuta un contenedor, utiliza las distintas capas de una imagen para su sistema de archivos.
Cada contenedor también tiene su propio "espacio temporal" para crear/actualizar/eliminar archivos. Cualquier cambio no se verán en otro contenedor, incluso si usan la misma imagen.

### Veamos esto en práctica

Para ver esto en acción, iniciará dos contenedores. En un contenedor, creará un archivo. En el otro contenedor, verificará que el archivo exista.
Lo que verás es que el archivo creado en un contenedor no está disponible en otro.

1. Iniciar un contenedor de Alpine y accede mediante shell.

    ```console
    $ docker run -ti --name=mytest alpine
    ```

2. En el contenedor crea un archivo llamado `greeting.txt` y escribe `hello`.

    ```console
    / # echo "hello" > greeting.txt
    ```

3. Sal del contenedor.

   ```console
   / # exit
   ```

4. Inicia un nuevo contenedor de Alpine y usa el comando `cat` para verificar que el archivo no existe.
   
   ```console
   $ docker run alpine cat greeting.txt
   ```

   Debería ver un resultado similar al siguiente que indica que el archivo no existe en el nuevo contenedor.

   ```console
   cat: can't open 'greeting.txt': No such file or directory
   ```

5. Continúe y elimine los contenedores usando `docker ps --all` para obtenerlas ID's, y luego `docker rm -f <container-id>` para eliminar los contenedores.


## Volúmenes de contenedores

Con el experimento anterior, viste que cada contenedor comienza desde la definición de la imagen cada vez que comienza.
Si bien los contenedores pueden crear, actualizar y eliminar archivos, esos cambios se pierden cuando elimina el contenedor y Docker aísla todos los cambios en ese contenedor. Con los volúmenes, puedes cambiar todo esto.

Los `volúmenes` brindan la capacidad de conectar rutas específicas del sistema de archivos de el contenedor de regreso a la máquina host. Si monta un directorio en el contenedor, los cambios en ese directorio también se ve en la máquina host. Si monta ese mismo directorio en los reinicios del contenedor, verá los mismos archivos.

Hay dos tipos principales de volúmenes. Eventualmente usarás ambos, pero comenzarás con montajes de volumen.

## Persistencia de los datos

De forma predeterminada, la aplicación de to-do almacena sus datos en una base de datos SQLite en `/etc/todos/todo.db` en el sistema de archivos del contenedor.

Dado que la base de datos es un solo archivo, si puede conservar ese archivo en el host y ponerlo a disposición del siguiente contenedor, debería poder continuar donde lo dejó el último. Creando un volumen y adjuntando (a menudo llamado "montaje") en el directorio donde almacenó los datos, puede conservar los datos. Como tu contenedor escribe en el archivo `todo.db`, ​​los datos persistirán en el host en el volumen.

Como se mencionó, vas a utilizar un soporte de volumen. Piense en un montaje de volumen como un depósito de datos. Docker administra completamente el volumen, incluida la ubicación de almacenamiento en el disco. Sólo necesitas recordar el
nombre del volumen.

### Crear el volúmen y iniciar el contenedor

1. Cree un volumen utilizando el comando `docker volume create`.

   ```console
   $ docker volume create todo-db
   ```

2. Detenga y elimine el contenedor de la aplicación de tareas pendientes nuevamente con `docker rm -f <id>`,
 ya que todavía se está ejecutando sin utilizar el volumen persistente.

3. Inicie el contenedor de la aplicación de tareas pendientes, pero agregue la opción `--mount` para especificar un
 montaje de volumen. Asigne un nombre al volumen y móntelo en `/etc/todos` en el
 contenedor, que captura todos los archivos creados en la ruta.

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started

### Verificar que los datos persistan

1. Una vez que se inicie el contenedor, abra la aplicación y agregue algunos elementos a su lista de tareas pendientes.

2. Detenga y elimine el contenedor de la aplicación de tareas pendientes. Utilice Docker Desktop o `docker ps` para obtener el ID y luego `docker rm -f <id>` para eliminarlo.

3. Inicie un nuevo contenedor siguiendo los pasos anteriores.

4. Abra la aplicación. Deberías ver tus artículos todavía en tu lista.

5. Continúe y retire el contenedor cuando haya terminado de revisar su lista.

## Sumérgete en el volumen

Mucha gente pregunta con frecuencia "¿Dónde almacena Docker mis datos cuando uso un volumen?" Si tu quieres saber,
puede utilizar el comando `docker volume inspect`.

```console
$ docker volume inspect todo-db
```

Verás una salida como esta:
```console
[
    {
        "CreatedAt": "2019-09-26T02:18:36Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": {},
        "Scope": "local"
    }
]
```

El `Mountpoint` es la ubicación real de los datos en el disco. Tenga en cuenta que en la mayoría de las máquinas, deberá necesitar tener acceso de root para acceder a este directorio desde el host.


## Montaje enlazado

Utilizaste un montaje de volumen para conservar los datos en su base de datos. Un montaje de volumen es una excelente opción cuando necesita un lugar persistente para almacenar los datos de su aplicación.

Un montaje enlazado es otro tipo de montaje que le permite compartir un directorio desde el sistema de archivos del host en el contenedor. Cuando trabaje en una aplicación, puede use un montaje de enlace para montar el código fuente en el contenedor. El contenedor ven loos cambios que realiza en el código inmediatamente, tan pronto como guarda un archivo. Esto significa
que puede ejecutar procesos en el contenedor que vigilan los cambios en el sistema de archivos y responderles.

En este capítulo, verá cómo puede utilizar montajes de enlace y una herramienta llamada [nodemon](https://npmjs.com/package/nodemon) para observar los cambios en el archivo y luego reiniciar la aplicación automáticamente. Existen herramientas equivalentes en la mayoría de los demás idiomas y
marcos.

## Comparaciones rápidas de tipos de volumen

Los siguientes son ejemplos de un volumen con nombre y un montaje vinculado usando `--mount`:

- Volumen con nombre: `type=volume,src=my-volume,target=/usr/local/data`
- Montaje de enlace: `type=bind,src=/path/to/data,target=/usr/local/data`

La siguiente tabla describe las principales diferencias entre montajes de volumen y monturas vinculadas.

|                                              | Volúmen                                      | Enlazado                                          |
| -------------------------------------------- | -------------------------------------------------- | ---------------------------------------------------- |
| Dirección en el equipo.                                | Docker elíge                                     | Tu decides                                           |
| Llena un nuevo volumen con el contenido del contenedor. | Si                                                | No                                                   |
| Admite controladores de volumen                      | Si                                                | No                                                   |

## Probando montajes enlazados

Antes de ver cómo puede utilizar montajes vinculantes para desarrollar su aplicación, puedes realizar un experimento rápido para obtener una comprensión práctica de cómo se monta.

1. Verifique que su directorio `getting-started-app` esté en un directorio definido en configuración para compartir archivos. Esta configuración define qué partes de su sistema de archivos que puedes compartir con contenedores.

2. Abra una terminal y cambie el directorio a `getting-started-app`.

3. Ejecute el siguiente comando para iniciar `bash` en un contenedor `ubuntu` con un enlaze de montaje.

   ```console
   $ docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash
   ```
   
    La opción `--mount type=bind` le dice a Docker que cree un montaje de enlace, donde `src` es el
    directorio de trabajo actual en su máquina host (`getting-started-app`), y
    `target` es donde ese directorio debería aparecer dentro del contenedor (`/src`).

4. Después de ejecutar el comando, Docker inicia una sesión interactiva `bash` en el
 directorio raíz del sistema de archivos del contenedor.

   ```console
   root@ac1237fad8db:/# pwd
   /
   root@ac1237fad8db:/# ls
   bin   dev  home  media  opt   root  sbin  srv  tmp  var
   boot  etc  lib   mnt    proc  run   src   sys  usr
   ```

5. Cambie el directorio al directorio `src`.

    Este es el directorio que montó al iniciar el contenedor. Listadando el contenido de este directorio muestra los mismos archivos que en el
    directorio `getting-started-app` en su máquina host.

   ```console
   root@ac1237fad8db:/# cd src
   root@ac1237fad8db:/src# ls
   Dockerfile  node_modules  package.json  spec  src  yarn.lock
   ```

6. Crea un nuevo archivo `myfile.txt`.

   ```console
   root@ac1237fad8db:/src# touch myfile.txt
   root@ac1237fad8db:/src# ls
   Dockerfile  myfile.txt  node_modules  package.json  spec  src  yarn.lock
   ```

7. Abra el directorio `getting-started-app` en el host y observe que el archivo `myfile.txt` está en el directorio.

   ```text
   ├── getting-started-app/
   │ ├── Dockerfile
   │ ├── myfile.txt
   │ ├── node_modules/
   │ ├── package.json
   │ ├── spec/
   │ ├── src/
   │ └── yarn.lock
   ```

8. Desde el host, elimine el archivo `myfile.txt`.
9. En el contenedor, enumere el contenido del directorio `app` una vez más observe que el archivo ya no está.

   ```console
   root@ac1237fad8db:/src# ls
   Dockerfile  node_modules  package.json  spec  src  yarn.lock
   ```

10. Detenga la sesión del contenedor interactivo con `Ctrl` + `D`.

Eso es todo para una breve introducción a las monturas vinculantes. Este procedimiento demostró cómo se comparten los archivos entre el host y el contenedor, y cómo los cambios se reflejan inmediatamente en ambos lados.


## Contenedores de desarrollo

El uso de montajes enlazados es común para las configuraciones de desarrollo local. La ventaja es que la máquina de desarrollo no necesita tener instalados todos los entornos y herramientas de compilación. Con un solo comando de ejecución de Docker, Docker extrae dependencias y herramientas.

### Ejecute su aplicación en un contenedor de desarrollo

Los siguientes pasos describen cómo ejecutar un contenedor de desarrollo con un enlace montaje que hace lo siguiente:

- Monte su código fuente en el contenedor.
- Instalar todas las dependencias.
- Inicie `nodemon` para observar los cambios en el sistema de archivos.

1. Asegúrese de no tener ningún contenedor de introducción en ejecución actualmente.

2. Ejecute el siguiente comando desde el directorio `getting-started-app` 

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 \
       -w /app --mount type=bind,src="$(pwd)",target=/app \
       node:18-alpine \
       sh -c "yarn install && yarn run dev"
   ```

    El siguiente es un desglose del comando:
    - `-dp 127.0.0.1:3000:3000` - igual que antes. Ejecutar en modo independiente (en segundo plano) y crear un mapeo de puertos
    - `-w /app` - establece el "directorio de trabajo" o el directorio actual donde el comando se ejecutará desde
    - `--mount type=bind,src="$(pwd)",target=/app` - enlaza el actual
    directorio desde el host al directorio `/app` en el contenedor
    - `node:18-alpine`: la imagen a utilizar. Tenga en cuenta que esta es la imagen base para su aplicación desde el Dockerfile
    - `sh -c "yarn install && Yarn run dev"` - el comando. Estás empezando un
    shell usando `sh` (alpine no tiene `bash`) y ejecutando `yarn install` para
    instale paquetes y luego ejecute `yarn run dev` para iniciar el desarrollo
    servidor. Si miras en `package.json`, verás que el script `dev` inicia `nodemon`.

3. Puede ver los registros usando `docker logs <container-id>`. Sabrás que      estás listo para comenzar cuando veas esto:  

   ```console
   $ docker logs -f <container-id>
   nodemon -L src/index.js
   [nodemon] 2.0.20
   [nodemon] to restart at any time, enter `rs`
   [nodemon] watching path(s): *.*
   [nodemon] watching extensions: js,mjs,json
   [nodemon] starting `node src/index.js`
   Using sqlite database at /etc/todos/todo.db
   Listening on port 3000
   ```

### Desarrolla tu aplicación con el contenedor de desarrollo

Actualice su aplicación en su máquina host y vea los cambios reflejados en el contenedor.

1. En el archivo `src/static/js/app.js`, en línea 109, cambie el botón "Agregar elemento" para que diga simplemente "Agregar":

   ```diff
   - {submitting ? 'Adding...' : 'Add Item'}
   + {submitting ? 'Adding...' : 'Add'}
   ```

   Guarda el archivo.

2. Actualice la página en su navegador web y debería ver el cambio reflejado.
 casi de inmediato debido al soporte de enlace. Nodemon detecta el cambio y
 reinicia el servidor. Es posible que el servidor de Node tarde unos segundos en
 reanudar. Si recibe un error, intente actualizar después de unos segundos.

3. Siéntete libre de realizar cualquier otro cambio que desees. Cada vez que    haces un cambiar y guardar un archivo, el cambio se refleja en el contenedor debido a el soporte de enlace. Cuando Nodemon detecta un cambio, reinicia la aplicación dentro del contenedor automáticamente. Cuando haya terminado, detenga el contenedor y construya su nueva imagen usando:

   ```console
   $ docker build -t getting-started .
   ```

#  Multi-Contenedores

Hasta este punto, ha estado trabajando con aplicaciones de contenedor único. Pero ahora agregará MySQL al pila de aplicaciones. A menudo surge la siguiente pregunta: "¿Dónde se ejecutará MySQL? ¿Instalarlo en el mismo contenedor o ejecutarlo por separado?" En general, cada contenedor debe hacer una cosa y hacerlo bien. Las siguientes son algunas razones para ejecutar el contenedor por separado:

- Es muy probable que tengas que escalar las API y las interfaces de usuario de forma diferente a las bases de datos.
- Los contenedores separados le permiten versionar y actualizar versiones de forma aislada.
- Si bien puedes usar un contenedor para la base de datos localmente, es posible que desees usar un servicio administrado.
 para la base de datos en producción. Entonces no querrás enviar tu motor de base de datos con tu aplicación.
- La ejecución de múltiples procesos requerirá un administrador de procesos (el contenedor solo inicia un proceso), lo que agrega complejidad al inicio/apagado del contenedor.

Y hay más razones. Entonces, como en el siguiente diagrama, es mejor ejecutar su aplicación en varios contenedores.

![Todo App connected to MySQL container](images/multi-container.webp?w=350h=250)


## Redes de contenedores

Recuerde que los contenedores, por defecto, se ejecutan de forma aislada y no saben nada sobre otros procesos o contenedores en la misma máquina. Entonces, ¿cómo se permite que un contenedor hable con otro? La respuesta es redes. Si coloca los dos contenedores en la misma red, pueden comunicarse entre sí.

## Iniciar MySQL

Hay dos formas de colocar un contenedor en una red:
 - Asignar la red al iniciar el contenedor.
 - Conectar un contenedor que ya se está ejecutando a una red.

En los siguientes pasos, primero creará la red y luego adjuntará el contenedor MySQL al inicio.

1. Crear la red.

   ```console
   $ docker network create todo-app
   ```

2. Inicie un contenedor MySQL y conéctelo a la red. También vas a definir algunas variables de entorno que la base de datos utilizará para inicializar la base de datos. Para obtener más información sobre las variables de entorno de MySQL, consulte la sección "Variables de entorno" en el [listado de MySQL Docker Hub](https://hub.docker.com/_/mysql/).
   
   ```console
   $ docker run -d \
       --network todo-app --network-alias mysql \
       -v todo-mysql-data:/var/lib/mysql \
       -e MYSQL_ROOT_PASSWORD=secret \
       -e MYSQL_DATABASE=todos \
       mysql:8.0
   ```
   En el comando anterior, puede ver el indicador `--network-alias`. En una sección posterior, aprenderá más sobre esta bandera.

   > **Tip**
   >
   > Notarás un volumen llamado `todo-mysql-data` en el comando anterior que está montado en `/var/lib/mysql`, que es donde MySQL almacena sus datos. Sin embargo, nunca ejecutó un comando "crear volumen de ventana acoplable". Docker reconoce que desea utilizar un volumen con nombre y crea uno automáticamente para usted.

3. Para confirmar que tiene la base de datos en funcionamiento, conéctese a la base de datos y verifique que se conecte.

   ```console
   $ docker exec -it <mysql-container-id> mysql -u root -p
   ```

    Cuando aparezca la solicitud de contraseña, escriba "secret". En el shell MySQL, enumere las bases de datos y verifique verá la base de datos `todos`.

   ```console
   mysql> SHOW DATABASES;
   ```

   Tu veras una impresión como la siguiente:

   ```plaintext
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | performance_schema |
   | sys                |
   | todos              |
   +--------------------+
   5 rows in set (0.00 sec)
   ```

4. Salga del shell MySQL para regresar al shell de su máquina.

   ```console
   mysql> exit
   ```

   Ahora tiene una base de datos `todos` y está lista para usar.

## Conéctate a MySQL

Ahora que sabes que MySQL está funcionando, puedes usarlo. Pero, ¿cómo se usa? Si tu corres otro contenedor en la misma red, ¿cómo encuentras el contenedor? Recuerda que cada contenedor tiene su propia dirección IP.

Para responder las preguntas anteriores y comprender mejor las redes de contenedores, utilizará el contenedor [nicolaka/netshoot](https://github.com/nicolaka/netshoot), que viene con muchas herramientas que son útiles para solucionar problemas o depurar problemas de red.

1. Inicie un nuevo contenedor usando la imagen nicolaka/netshoot. Asegúrate de conectarlo a la misma red.

   ```console
   $ docker run -it --network todo-app nicolaka/netshoot
   ```

2. Dentro del contenedor, utilizará el comando "dig", que es una herramienta DNS útil. vas a mirar hacia arriba la dirección IP para el nombre de host `mysql`.

   ```console
   $ dig mysql
   ```

    Tu veras una impresión como la siguiente:

   ```text
   ; <<>> DiG 9.18.8 <<>> mysql
   ;; global options: +cmd
   ;; Got answer:
   ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32162
   ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

   ;; QUESTION SECTION:
   ;mysql.				IN	A

   ;; ANSWER SECTION:
   mysql.			600	IN	A	172.23.0.2

   ;; Query time: 0 msec
   ;; SERVER: 127.0.0.11#53(127.0.0.11)
   ;; WHEN: Tue Oct 01 23:47:24 UTC 2019
   ;; MSG SIZE  rcvd: 44
   ```
    En la "ANSWER SECTION", verá un registro "A" para "mysql" que se resuelve en "172.23.0.2". (Lo más probable es que su dirección IP tenga un valor diferente). Si bien `mysql` normalmente no es un nombre de host válido, Docker pudo resolverlo en la dirección IP del contenedor que tenía ese alias de red. Recuerda que usaste el `--network-alias` anteriormente.

    Lo que esto significa es que su aplicación simplemente necesita conectarse a un host llamado `mysql` y se comunicará con la base de datos.

## Ejecute su aplicación con MySQL

La aplicación Todo admite la configuración de algunas variables de entorno para especificar la configuración de conexión MySQL. Ellos son:

- `MYSQL_HOST`: el nombre de host del servidor MySQL en ejecución
- `MYSQL_USER`: el nombre de usuario que se utilizará para la conexión
- `MYSQL_PASSWORD` - la contraseña a utilizar para la conexión
- `MYSQL_DB` - la base de datos que se utilizará una vez conectado

> **Nota**
>
> Aunque generalmente se acepta para el desarrollo el uso de env vars para establecer la configuración de conexión, se desaconseja encarecidamente
> al ejecutar aplicaciones en producción. Diogo Mónica, exjefe de seguridad de Docker,
> [escribió una publicación de blog fantástica](https://diogomonica.com/2017/03/27/why-you-shouldnt-use-env-variables-for-secret-data/)
> explicando por qué.
>
> Un mecanismo más seguro es utilizar el soporte secreto proporcionado por su marco de orquestación de contenedores. En la mayoría de los casos,
> estos secretos se montan como archivos en el contenedor en ejecución. Verás muchas aplicaciones (incluida la imagen de MySQL y la aplicación de tareas pendientes)
> también admite variables de entorno con un sufijo `_FILE` para apuntar a un archivo que contiene la variable.
>
> Como ejemplo, configurar la var `MYSQL_PASSWORD_FILE` hará que la aplicación use el contenido del archivo al que se hace referencia
> como contraseña de conexión. Docker no hace nada para admitir estas variables de entorno. Tu aplicación necesitará saberlo para buscar
> la variable y obtener el contenido del archivo.

Ahora puede iniciar su contenedor listo para desarrollo.

1. Especifique cada una de las variables de entorno anteriores, así como también conecte el contenedor a la red de su aplicación. Asegúrese de estar en el directorio `getting-started-app` cuando ejecute este comando.

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 \
     -w /app -v "$(pwd):/app" \
     --network todo-app \
     -e MYSQL_HOST=mysql \
     -e MYSQL_USER=root \
     -e MYSQL_PASSWORD=secret \
     -e MYSQL_DB=todos \
     node:18-alpine \
     sh -c "yarn install && yarn run dev"
   ```

2. Si observa los registros del contenedor (`docker logs -f <container-id>`), debería ver un mensaje similar al siguiente, que indica que es usando la base de datos mysql.

   ```console
   $ nodemon src/index.js
   [nodemon] 2.0.20
   [nodemon] to restart at any time, enter `rs`
   [nodemon] watching dir(s): *.*
   [nodemon] starting `node src/index.js`
   Connected to mysql db at host mysql
   Listening on port 3000
   ```
3. Abra la aplicación en su navegador y agregue algunos elementos a su lista de tareas pendientes.

4. Conéctese a la base de datos mysql y demuestre que los elementos se están escribiendo en la base de datos. Recuerda, la contraseña es "secret".

   ```console
   $ docker exec -it <mysql-container-id> mysql -p todos
   ```

   Y en el shell de mysql, ejecuta lo siguiente:

   ```console
   mysql> select * from todo_items;
   +--------------------------------------+--------------------+-----------+
   | id                                   | name               | completed |
   +--------------------------------------+--------------------+-----------+
   | c906ff08-60e6-44e6-8f49-ed56a0853e85 | Do amazing things! |         0 |
   | 2912a79e-8486-4bc3-a4c5-460793a575ab | Be awesome!        |         0 |
   +--------------------------------------+--------------------+-----------+
   ```

Es muy probable que estés empezando a sentirte un poco abrumado con todo lo que necesitas hacer para comenzar esta aplicación. Tienes que crear una red, iniciar contenedores, especificar todas las variables de entorno, exponer puertos y más. Hay mucho que recordar y ciertamente hace que sea más difícil transmitirlo a otra persona.

En la siguiente sección, aprenderá sobre Docker Compose. Con Docker Compose, puede compartir sus pilas de aplicaciones en un
manera mucho más fácil y deja que otros los hagan girar con un comando único y simple.
