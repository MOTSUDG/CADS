
# Docker Compose
<p align=center>
<img src="images/compose/1.png" width="800" >
</p>  

Docker Compose es una herramienta para definir y ejecutar aplicaciones de múltiples contenedores. Es la clave para desbloquear una experiencia de desarrollo e implementación optimizada y eficiente.

Compose simplifica el control de toda su pila de aplicaciones, lo que facilita la administración de servicios, redes y volúmenes en un archivo de configuración YAML único y comprensible. Luego, con un solo comando, crea e inicia todos los servicios desde su archivo de configuración.

Componer obras en todos los ambientes; producción, puesta en escena, desarrollo, pruebas y flujos de trabajo de CI. También tiene comandos para gestionar todo el ciclo de vida de su aplicación:

Iniciar, detener y reconstruir servicios
Ver el estado de los servicios en ejecución
Transmita la salida del registro de los servicios en ejecución
Ejecutar un comando único en un servicio

## Beneficios clave de Docker Compose

El uso de Docker Compose ofrece varios beneficios que agilizan el desarrollo, la implementación y la administración de aplicaciones en contenedores:

- Control simplificado: Docker Compose le permite definir y administrar aplicaciones de múltiples contenedores en un solo archivo YAML. Esto simplifica la compleja tarea de orquestar y coordinar varios servicios, lo que facilita la gestión y replicación de su entorno de aplicaciones.

- Colaboración eficiente: Los archivos de configuración de Docker Compose son fáciles de compartir, lo que facilita la colaboración entre desarrolladores, equipos de operaciones y otras partes interesadas.

- Desarrollo rápido de aplicaciones: Compose almacena en caché la configuración utilizada para crear un contenedor. Cuando reinicia un servicio que no ha cambiado, Compose reutiliza los contenedores existentes. Reutilizar contenedores significa que puede realizar cambios en su entorno muy rápidamente.

- Portabilidad entre entornos: Compose admite variables en el archivo Compose. Puede utilizar estas variables para personalizar su composición para diferentes entornos o diferentes usuarios.

# Como funciona Docker Compose

<p align=center>
<img src="images/compose/2.jpg" width="800" >
</p>

Docker Compose se basa en un archivo de configuración YAML, normalmente denominado compose.yaml.

La ruta predeterminada para un archivo de redacción es compose.yaml(preferida) que se encuentra en el directorio de trabajo. 

### Ejemplo ilustrativo

Considere una aplicación dividida en una aplicación web frontend y un servicio backend.

- La interfaz se configura en tiempo de ejecución con un archivo de configuración HTTP administrado por la infraestructura, que proporciona un nombre de dominio externo y un certificado de servidor HTTPS inyectado por el almacén secreto seguro de la plataforma.

El backend almacena datos en un volumen persistente.

Ambos servicios se comunican entre sí en una red de nivel posterior aislada, mientras que el frontend también está conectado a una red de nivel frontal y expone el puerto 443 para uso externo.

La aplicación de ejemplo se compone de las siguientes partes:

- 2 servicios, respaldados por imágenes de Docker: webappydatabase
- 1 secreto (certificado HTTPS), inyectado en el frontend
- 1 configuración (HTTP), inyectada en el frontend
- 1 volumen persistente, adjunto al backend
- 2 redes

<p align=center>
<img src="images/compose/3.webp" width="1000" >
</p>

```yaml
services:
  frontend:
    image: example/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate
  backend:
    image: example/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier
volumes:
  db-data:
    driver: flocker
    driver_opts:
      size: "10GiB"
configs:
  httpd-config:
    external: true
secrets:
  server-certificate:
    external: true
networks:
  # The presence of these objects is sufficient to define them
  front-tier: {}
  back-tier: {}
```

# Construir aplicación web con Docker Compose

En el `getting-started-app` directorio, cree un archivo llamado `compose.yaml`.

```console
├── getting-started-app/
│ ├── Dockerfile
│ ├── compose.yaml
│ ├── node_modules/
│ ├── package.json
│ ├── spec/
│ ├── src/
│ └── yarn.lock
```

En una sección utilizamos un comando para iniciar el servicio de la aplicación.

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

Ahora definirá este servicio en el archivo `compose.yaml`.

1. Ábra el archivo `compose.yaml` en un editor de texto o código y comience definiendo el nombre y la imagen del primer servicio (o contenedor) que desea ejecutar como parte de su aplicación. El nombre se convertirá automáticamente en un alias de red, lo que será útil al definir su servicio MySQL.

    ```yaml
    services:
      app:
        image: node:18-alpine
    ```

2. Por lo general, verá `command` cerca de la imagen, agregue el `command` a su `compose.yaml`.

    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
    ```
3. Ahora migre la `-p 127.0.0.1:3000:3000` hacia el comando definiendo el ports para el servicio.

    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 127.0.0.1:3000:3000
    ```

4. A continuación, migre tanto el directorio de trabajo 
`( -w /app)` como la asignación de volumen `( -v "$(pwd):/app")` utilizando las definiciones `working_dir` y `volumes`.

    Una ventaja de las definiciones de volúmenes de Docker Compose es que puede utilizar rutas relativas desde el directorio actual.

    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 127.0.0.1:3000:3000
        working_dir: /app
        volumes:
          - ./:/app
    ```

  5. Finalmente, necesita migrar las definiciones de variables de entorno usando la clave `environment`.

      ```yaml
      services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 127.0.0.1:3000:3000
        working_dir: /app
        volumes:
          - ./:/app
        environment:
          MYSQL_HOST: mysql
          MYSQL_USER: root
          MYSQL_PASSWORD: secret
          MYSQL_DB: todos
      ```

# Definir el servicio MySQL

Ahora es el momento de definir el servicio MySQL. El comando que usaste para ese contenedor fue el siguiente:

```consola
$ docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:8.0
```

1. Primero defina el nuevo servicio y asígnele un nombre mysqlpara que obtenga automáticamente el alias de red. También especifique la imagen que se utilizará.

    ```yaml
      services:
        app:
          # The app service definition
        mysql:
          image: mysql:8.0
    ```

2. A continuación, defina la asignación de volumen. Cuando ejecutó el contenedor con docker run, Docker creó el volumen nombrado automáticamente. Sin embargo, eso no sucede cuando se ejecuta con Compose. Debe definir el volumen en la sección de volúmenes y luego especificar el punto de montaje en la configuración del servicio. Simplemente proporcionando solo el nombre del volumen, se utilizan las opciones predeterminadas.

    ```yaml
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
        volumes:
          - todo-mysql-data:/var/lib/mysql

    volumes:
      todo-mysql-data:```

3. Finalmente, debe especificar las variables de entorno.

    ```yaml
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
        volumes:
          - todo-mysql-data:/var/lib/mysql
        environment:
          MYSQL_ROOT_PASSWORD: secret
          MYSQL_DATABASE: todos

    volumes:
      todo-mysql-data:

En este punto, su completo compose.yamldebería verse así:

```yaml
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

# Ejecute la pila de aplicaciones

Ahora que tiene su compose.yaml, puede iniciar su solicitud.

1. Asegúrese de que no se estén ejecutando otras copias de los contenedores primero. Úse `docker ps` para enumerar los contenedores y docker rm -f <-ids-> para eliminarlos.

2. Inicie la pila de aplicaciones usando el `docker compose up` comando. Agregue la bandera -d para ejecutar todo en segundo plano.

    ```console
    $ docker compose up -d
    ```

    Cuando ejecuta el comando anterior, debería ver un resultado como el siguiente:


    ```console
    Creating network "app_default" with the default driver
    Creating volume "app_todo-mysql-data" with default driver
    Creating app_app_1   ... done
    Creating app_mysql_1 ... done
    ```

    Notarás que Docker Compose creó el volumen y también una red. De forma predeterminada, Docker Compose crea automáticamente una red específica para la pila de aplicaciones (razón por la cual no definió una en el archivo Compose).

3. Mire los registros usando `docker compose logs -f`. Verá los registros de cada uno de los servicios entrelazados en una sola secuencia. Esto es increíblemente útil cuando desea estar atento a problemas relacionados con el tiempo. La `-f` bandera sigue el registro, por lo que le brindará resultados en vivo a medida que se genera.

    Si ya ejecutó el comando, verá un resultado similar a este:


    ```console
    mysql_1  | 2019-10-03T03:07:16.083639Z 0 [Note] mysqld: ready for connections.
    mysql_1  | Version: '8.0.31'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)
    app_1    | Connected to mysql db at host mysql
    app_1    | Listening on port 3000
    ```

    El nombre del servicio se muestra al principio de la línea (a menudo en color) para ayudar a distinguir los mensajes. Si desea ver los registros de un servicio específico, puede agregar el nombre del servicio al final del comando de registros (por ejemplo, docker compose logs -f app).

4. En este punto, debería poder abrir su aplicación en su navegador en http://localhost:3000 y verla ejecutándose.

5. Finalmente detenga su aplicación con el comando `docker compose down`