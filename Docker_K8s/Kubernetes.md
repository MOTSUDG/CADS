# Introducción a Kubernetes 

<p align=center>
<img src="images/k8s/1.webp" width="500" >
</p>

Kubernetes es una plataforma portátil, extensible y de código abierto para gestionar cargas de trabajo y servicios en contenedores, que facilita tanto la configuración declarativa como la automatización. Tiene un ecosistema grande y de rápido crecimiento. Los servicios, el soporte y las herramientas de Kubernetes están ampliamente disponibles.

# Alcance de este curso:

- Retrocidiendo en el tiempo
- Arquitectura
- Instalación
- Contextos
- Imperativo vs Declarativo
- Namespaces
- Pod
- Selectores
- Pods Multi-Contenedores
- ReplicaSet
- Deployments
- Stateful
- Job
- CronJob
- Rolling Updates
- Despliegue azul y verde
- ClusterIP
- NodePort
- LoadBalancer
- Persistence Volumen
- ConfigMap
- Secrets
- LivenessProbe

# Retrocediendo en el timepo

<p align=center>
<img src="images/k8s/2.png" width="600" >
</p>

 - **Era de implementación tradicional**: Al principio, las organizaciones ejecutaban aplicaciones en servidores físicos. No había forma de definir límites de recursos para las aplicaciones en un servidor físico y esto causaba problemas de asignación de recursos. Por ejemplo, si se ejecutan varias aplicaciones en un servidor físico, puede haber casos en los que una aplicación consumiría la mayor parte de los recursos y, como resultado, las otras aplicaciones tendrían un rendimiento inferior. Una solución para esto sería ejecutar cada aplicación en un servidor físico diferente. Pero esto no creció porque los recursos estaban infrautilizados y a las organizaciones les resultaba costoso mantener muchos servidores físicos.

 - **Era de la implementación virtualizada**: Como solución se introdujo la virtualización. Le permite ejecutar múltiples máquinas virtuales (VM) en la CPU de un único servidor físico. La virtualización permite aislar las aplicaciones entre máquinas virtuales y proporciona un nivel de seguridad ya que otra aplicación no puede acceder libremente a la información de una aplicación. 
 
    La virtualización permite una mejor utilización de los recursos en un servidor físico y permite una mejor escalabilidad porque una aplicación se puede agregar o actualizar fácilmente, reduce los costos de hardware y mucho más. Con la virtualización puedes presentar un conjunto de recursos físicos como un grupo de máquinas virtuales desechables.

    Cada VM es una máquina completa que ejecuta todos los componentes, incluido su propio sistema operativo, sobre el hardware virtualizado.

 -  **Era de implementación de contenedores**: Los contenedores son   similares a las máquinas virtuales, pero tienen propiedades de  aislamiento relajadas para compartir el sistema operativo (SO) entre las aplicaciones. Por tanto, los contenedores se consideran ligeros. Al igual que una máquina virtual, un contenedor tiene su propio sistema de archivos, CPU compartida, memoria, espacio de proceso y más. Como están desacoplados de la infraestructura subyacente, son portátiles a través de nubes y distribuciones de sistema operativo. 

# Arquitectura

Cuando se despliega Kubernetes, se obtiene un clúster.

Un clúster Kubernetes consiste en un conjunto de máquinas de trabajo, llamadas nodos, que ejecutan aplicaciones en contenedores. Cada clúster tiene al menos un nodo trabajador.

El nodo(s) trabajador(es) aloja(n) los Pods que son los componentes de la carga de trabajo de la aplicación. El plano de control gestiona los nodos trabajadores y los Pods en el cluster. En entornos de producción, el plano de control suele ejecutarse en varios ordenadores y un clúster suele ejecutar varios nodos, lo que proporciona tolerancia a fallos y alta disponibilidad.

<p align=center>
<img src="images/k8s/3.svg" width="1000" >
</p>

### Componentes del plano de control

Los componentes del plano de control toman decisiones globales sobre el clúster (por ejemplo, programación), así como detectan y responden a eventos del clúster (por ejemplo, iniciar un nuevo pod cuando el campo de réplicas de un despliegue no está satisfecho).

Los componentes del plano de control pueden ejecutarse en cualquier máquina del cluster. Sin embargo, por simplicidad, los scripts de configuración normalmente inician todos los componentes del plano de control en la misma máquina, y no ejecutan contenedores de usuario en esta máquina. Vea Creando clusters altamente disponibles con kubeadm para un ejemplo de configuración del plano de control que corre a través de múltiples máquinas.

### - kube-apiserver
El servidor API es un componente del plano de control de Kubernetes que expone la API de Kubernetes. El servidor API es la interfaz del plano de control de Kubernetes.

La principal implementación de un servidor API de Kubernetes es kube-apiserver. kube-apiserver está diseñado para escalar horizontalmente, es decir, escala desplegando más instancias. Puede ejecutar varias instancias de kube-apiserver y equilibrar el tráfico entre esas instancias.

### - etcd
Almacén de valores clave consistente y de alta disponibilidad utilizado como almacén de respaldo de Kubernetes para todos los datos del clúster.

### - kube-scheduler
Componente del plano de control que busca Pods recién creados sin nodo asignado, y selecciona un nodo para que se ejecuten.

### - kube-controller-manager
Componente del plano de control que ejecuta procesos de controlador. Lógicamente, cada controlador es un proceso independiente, pero para reducir la complejidad, todos se compilan en un único binario y se ejecutan en un único proceso.

Hay muchos tipos diferentes de controladores. Algunos ejemplos son:

- Controlador de nodos: Se encarga de avisar y responder cuando los nodos se caen.
- Controlador de trabajos: Busca objetos Job que representan tareas puntuales y crea Pods para ejecutar esas tareas hasta su finalización.
- Controlador EndpointSlice: Rellena los objetos EndpointSlice (para proporcionar un vínculo entre los servicios y los pods).
- Controlador ServiceAccount: Crea ServiceAccounts por defecto para nuevos espacios de nombres.

### Componentes de nodo
Los componentes de nodo se ejecutan en cada nodo, manteniendo los pods en ejecución y proporcionando el entorno de ejecución de Kubernetes.

### - kubelet
Un agente que se ejecuta en cada nodo del clúster. Se asegura de que los contenedores se están ejecutando en un Pod.

El kubelet toma un conjunto de PodSpecs que se proporcionan a través de diversos mecanismos y se asegura de que los contenedores descritos en esos PodSpecs se están ejecutando y en buen estado. El kubelet no gestiona contenedores que no hayan sido creados por Kubernetes.

### - kube-proxy
kube-proxy es un proxy de red que se ejecuta en cada nodo de su clúster, implementando parte del concepto de Servicio Kubernetes.

kube-proxy mantiene reglas de red en los nodos. Estas reglas de red permiten la comunicación de red a sus Pods desde sesiones de red dentro o fuera de su cluster.

kube-proxy utiliza la capa de filtrado de paquetes del sistema operativo si existe y está disponible. De lo contrario, kube-proxy reenvía el tráfico por sí mismo.

### - Runtime del contenedor
Componente fundamental que permite a Kubernetes ejecutar contenedores de forma eficaz. Se encarga de gestionar la ejecución y el ciclo de vida de los contenedores dentro del entorno Kubernetes.

# Instalación de Kubernetes

 Para crear un cluster de `Kubernetes` localmente utilizaremos `minikube`.  Es una herramienta de código abierto que permite a los usuarios crear y administrar clústeres de Kubernetes en entornos locales o virtuales. Facilita el desarrollo, las pruebas y el aprendizaje de Kubernetes al proporcionar un entorno local que simula las características de producción, sin requerir acceso a un clúster completo en la nube o en un entorno de producción.
 
 ```consola
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

$ minikube start

$ minikube dashboard
 ```

Probemos que la instalación de minikube fue exitosa:

    kubectl cluster-info

# Contextos

Los contextos son atajos que le permiten cambiar rápidamente entre diferentes configuraciones de clúster sin escribir comandos largos y tediosos. Los detalles del contexto se almacenan en el archivo kubeconfig , un archivo de configuración YAML utilizado por Kubernetes para autenticarse y conectarse al servidor API de Kubernetes. El archivo normalmente se encuentra en el directorio de inicio del usuario.

1. Obtengamos el contexto actual:

       kubectl config current-context

2. Listamos todos los contextos:

       kubectl config get-contexts

3. Usar un contexto:

       kubectl config use-context [contextName]

4. Renombrar contexto:

       kubectl config rename-context [old-name] [new-name]

5. Eliminar contexto:

       kubectl config delete-context [contextName]

# Declaraciones (Imperativa vs Declarativa)

Vamos a desplegar un contenedor Nginx usando ambos metodos

1. Imperativa

       kubectl create deployment mynginx1 --image=nginx

2. Creamos un archivo `deploy-example.yaml`

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
   name: mynginx2
   spec:
   replicas: 1
   selector:
      matchLabels:
         app: nginx
         env: prod
   template:
      metadata:
         labels:
         app: nginx
         env: prod
      spec:
         containers:
         - name: nginx
         image: nginx
         resources:
            requests:
               cpu: 100m
               memory: 128Mi
            limits:
               cpu: 250m
               memory: 256Mi        
         ports:
         - containerPort: 80
   ```

3. Declarativa

       kubectl create -f deploy-example.yaml

4. Limpiamos

   ```console
   kubectl delete deployment mynginx1
   kubectl delete deploy mynginx2
   ```

# Namespaces

<p align=center>
<img src="images/k8s/4.png" width="500" >
</p>

Los `namespaces` proporcionan un mecanismo para aislar grupos de recursos dentro de un único clúster. Los nombres de los recursos deben ser únicos dentro de un espacio de nombres, pero no entre espacios de nombres. El alcance basado en espacios de nombres solo se aplica a espacios de namespaces (por ejemplo, implementaciones, servicios, etc.) y no para objetos de todo el clúster (por ejemplo, StorageClass, nodos, PersistentVolumes, etc.)

Los `Pods` son un grupo de uno o más contenedores, con almacenamiento compartido y recursos de red, y una especificación sobre cómo ejecutar los contenedores. Los contenidos de un Pod siempre están ubicados y programados conjuntamente, y se ejecutan en un contexto compartido. Un Pod modela un "host lógico" específico de una aplicación: contiene uno o más contenedores de aplicaciones que están relativamente estrechamente acoplados.

1. Obtener los namespaces

   Verificamos cuales son los namespaces actuales.

       kubectl get namespaces
       kubectl get ns

2. Obtenemos la lista de los pods Get the pods list

       kubectl get pods

3. Obtendras diferentes Pods entre los diferentes namespaces.

       kubectl get pods --namespace=kube-system
       kubectl get pods -n kube-system

4. Cambiar namespace

   Cambia el namespace hacia otro y obten la lista de sus pods.

       kubectl config set-context --current --namespace=kube-system
       kubectl get pods

5. Ahora regresamos al namespace por defecto.

       kubectl config set-context --current --namespace=default
       kubectl get pods

6. Creamos y eliminamos un namespace

       kubectl create ns [name]
       kubectl get ns
       kubectl delete ns [name]

## Obtener informacion de los nodos

Consultar la lista de todos los nodos instalados.

    kubectl get nodes

Obtener información adicional sobre un nodo.

    kubectl describe node

# Pods

<p align=center>
<img src="images/k8s/5.png" width="500" >
</p>

Primero vamos a crear un nodo ejecutando un servidor Nginx de la forma imperativa.

1. Creamos el pod

       kubectl run mynginx --image=nginx

2. Obtenemos la lista de los pods en ejecucion

       kubectl get pods

3. Obtenemos mas informacionde los pods

       kubectl get pods -o wide
       kubectl describe pod mynginx

4. Eliminamos el pod

       kubectl delete pod mynginx

5. Creamos un pod ejecutando BusyBox y accedemos a su terminal

       kubectl run mybox --image=busybox -it -- /bin/sh

6. Listamos sus directorios y usamos el siguiente comando

       ls
       echo -n 'A Secret' | base64
       exit

7. Limpiamos

    kubectl delete pod mybox

### Crear un pod de la manera declarativa

1. Creamos nuestro archivo YAML.

      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
      name: myapp-pod
      labels:
         app: myapp
         type: front-end
      spec:
      containers:
      - name: nginx-container
         image: nginx
         resources:
            requests:
            cpu: 100m
            memory: 128Mi
            limits:
            cpu: 250m
            memory: 256Mi    
         ports:
         - containerPort: 80
            name: http
            protocol: TCP
         env:
         - name: DBCON
            value: myconnectionstring
      ```


2. Vamos a crear un nodo utilizando un archivo YAML.

       kubectl create -f myapp.yaml

3. Obtenemos informacion adicional

       kubectl get pods -o wide
       kubectl describe pod myapp-pod

4. Ingresamos a su terminal

       kubectl exec -it myapp-pod -- bash

5. Imprimimos la variable de entorno DBCON que agregamos en nuestro archivo YAML.

       echo $DBCON

6. Salimos de la terminal

    exit

## Limpiamos

    kubectl delete -f myapp.yaml

# Selectores

Las etiquetas son pares clave/valor que se adjuntan a objetos como los pods. Las etiquetas están destinadas a especificar atributos de identificación de objetos que son significativos y relevantes para los usuarios. Las etiquetas se pueden utilizar para organizar y seleccionar subconjuntos de objetos. Las etiquetas se pueden adjuntar a los objetos en el momento de la creación y posteriormente agregarlas y modificarlas en cualquier momento. Cada objeto puede tener definido un conjunto de etiquetas clave/valor. Cada clave debe ser única para un objeto determinado.

1. Creamos un archivo YAML.

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
   name: myapp-pod
   labels:
      app: myapp
      type: front-end
   spec:
   containers:
   - name: nginx-container
      image: nginx
      resources:
         requests:
         cpu: 100m
         memory: 128Mi
         limits:
         cpu: 250m
         memory: 256Mi    
      ports:
      - containerPort: 80
   ```

2. Ejecutar la aplicación

       kubectl apply -f myapp.yaml

3. Creamos el archivo YAML `myservice`

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
   name: myservice
   spec:
   ports:
   - port: 80
      targetPort: 80
   selector:
      app: myapp
      type: front-end
   ```

4. Desplegar el servicio

       kubectl apply -f myservice.yaml

      **¿Esta el servicio conectado con el pod?**

       kubectl get po -o wide
       kubectl get ep myservice

5. Abrir el puerto para el servicio

       kubectl port-forward service/myservice 8080:80

Abre tu navegador y dirigete a la direccion http://localhost:8080

6. Editamos el archivo YAML

   Cambiamos la etiqueta **app** por **myapp2** y guardamos los cambios.

7. Desplegamos los cambios

        kubectl apply -f myapp.yaml

8. Checamos el punto de acceso.

        kubectl get ep myservice

9. Abrimos el puerto para el servicio de nuevo

        kubectl port-forward service/myservice 8080:80

Abre tu navegador y dirigete a la direccion http://localhost:8080

 Limpiamos

        kubectl delete -f myservice.yaml
        kubectl delete -f myapp.yaml

# Pods Multi-Contenedores

1. Creamos el pod.
    
   Para eso primero creamos nuestro archivo YAML

      ```yaml
       apiVersion: v1
       kind: Pod
       metadata:
       name: two-containers
       spec:
       restartPolicy: Always
       containers:
       - name: mynginx
       image: nginx
       resources:
       requests:
              cpu: 100m
              memory: 128Mi
       limits:
              cpu: 250m
              memory: 256Mi    
       ports:
       - containerPort: 80
       - name: mybox
       image: busybox
       resources:
       requests:
              cpu: 100m
              memory: 128Mi
       limits:
              cpu: 250m
              memory: 256Mi    
       ports:
       - containerPort: 81
       command:
       - sleep
       - "3600"
      ```
       
       kubectl create -f two-containers.yaml

2. Obtenemos información sobre el pod

       kubectl get pods -o wide
       kubectl describe pod two-containers

3. Nos conectamos al contenedor de BusyBox.

       kubectl exec -it two-containers --container mybox -- /bin/sh

4.  Extraemos la pagina HTML que esta alojada en el contenedor de Nginx.

        wget -qO- localhost

5. Salimos del contenedor

       exit

6. Limpiamos

       kubectl delete -f two-containers.yaml --force --grace-period=0

# ReplicaSet

El propósito de un ReplicaSet es mantener un conjunto estable de réplicas de Pods ejecutándose en un momento dado. Como tal, a menudo se utiliza para garantizar la disponibilidad de una cantidad específica de Pods idénticos. 

Un ReplicaSet se define con campos, incluido un selector que especifica cómo identificar los Pods que puede adquirir, una cantidad de réplicas que indica cuántos Pods debe mantener y una plantilla de pod que especifica los datos de los nuevos Pods que debe crear para cumplir con el número. de criterios de réplicas. Luego, un ReplicaSet cumple su propósito al crear y eliminar Pods según sea necesario para alcanzar el número deseado. Cuando un ReplicaSet necesita crear nuevos Pods, utiliza su plantilla de Pod.


1. Creamos una ReplicaSet.

   Creamos un archivo YAML.

      ```yaml
       apiVersion: apps/v1
       kind: ReplicaSet
       metadata:
       name: rs-example
       spec:
       replicas: 3
       selector:
       matchLabels:
       app: nginx
       type: front-end
       template: 
       metadata:
       labels:
              app: nginx
              type: front-end
       spec:
       containers:
       - name: nginx
              image: nginx:alpine
              resources:
              requests:
              cpu: 100m
              memory: 128Mi
              limits:
              cpu: 250m
              memory: 256Mi 
              ports:
              - containerPort: 80
      ```

   Ahora ejecutamos aplicamos los cambios

       kubectl apply -f rs-example.yaml

2. Obtenemos la lista de los pods.

       kubectl get pods -o wide

3. Obtenemos el nombre de la ReplicaSet.

       kubectl get rs

4. Describimos la ReplicaSet

       kubectl describe rs rs-example

5. Limpiamos

       kubectl delete -f rs-example.yaml

# Deployments

Deployment en Kubernetes se refiere a un objeto que describe cómo se debe implementar y actualizar una aplicación en el clúster. Proporciona un enfoque declarativo para definir el estado deseado de la aplicación y permite que Kubernetes se encargue de llevar el estado actual al estado deseado de manera eficiente y confiable.

Simplifica el proceso de despliegue y actualización de aplicaciones, proporcionando una forma estandarizada y automatizada de administrar el ciclo de vida de las aplicaciones en un clúster.

1. Creamos nuestro Deployment.

   Creamos un archivo YAML.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-example
spec:
  replicas: 3
  revisionHistoryLimit: 3
  selector:
    matchLabels:
      app: nginx
      env: prod
  template:
    metadata:
      labels:
        app: nginx
        env: prod
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi        
        ports:
       - containerPort: 80
```

Ahora ejecutamos aplicamos los cambios

    kubectl apply -f deploy-example.yaml

2. Obtenemos la lista de nuestros pods.

       kubectl get pods -o wide
    
3. Describimos el pod.

       kubectl describe pod deploy-example

4. Obtenemos informacion del Deployment.

       kubectl get deploy
       kubectl describe deploy deploy-example

5. Obtenemos el nombre de una replica.

       kubectl get rs

6. Describimos nuestra ReplicaSet

       kubectl describe rs

7. Limpiamos

       kubectl delete -f deploy-example.yaml

# Stateful

`StatefulSet` es el objeto API de carga de trabajo que se utiliza para administrar aplicaciones con estado. Gestiona la implementación y el escalado de los Pods, *y ofrece garantías sobre el orden y la singularidad de estos Pods*.

Como un Despliegue, un StatefulSet administra Pods que se basan en una especificación de contenedor idéntica. A diferencia de una implementación, un StatefulSet mantiene una identidad fija para cada uno de sus Pods. Estos pods se crean a partir de la misma especificación, pero no son intercambiables: cada uno tiene un identificador persistente que mantiene durante cualquier reprogramación.

1. Creamos el despliegue.

   Creamos nuestro archivo YAML statefulset.yaml.

      ```yaml
       apiVersion: v1
       kind: Service
       metadata:
       name: nginx-headless
       labels:
       run: nginx-sts-demo
       spec:
       ports:
       - port: 80
       name: web
       clusterIP: None
       selector:
       run: nginx-sts-demo
       ---
       apiVersion: apps/v1
       kind: StatefulSet
       metadata:
       name: nginx-sts
       spec:
       serviceName: nginx-headless
       replicas: 3
       selector:
       matchLabels:
       run: nginx-sts-demo
       template:
       metadata:
       labels:
              run: nginx-sts-demo
       spec:
       containers:
       - name: nginx
              image: nginx
              volumeMounts:
              - name: www
              mountPath: /var/www/
       volumeClaimTemplates:
       - metadata:
       name: www
       spec:
       storageClassName: hostpath
       accessModes:
              - ReadWriteOnce
       resources:
              requests:
              storage: 10Mi
      ```

       kubectl apply -f statefulset.yaml

2. Obtén la lista de los pods.

       kubectl get pods -o wide

3. Obtén la lista de los PersistentVolumes.

       kubectl get pvc

3. Crear un archivo en `nginx-sts-2`

   Abre una sesión en `nginx-sts-2` y crea un archivo en la carpeta mapeada al volumen.

       kubectl exec nginx-sts-2 -it -- /bin/sh
       cd var/www
       echo Hello > hello.txt

4. Modifica la pagina web por defecto.

       cd /usr/share/nginx/html
       cat > index.html
       Hello
       Ctrl-D
       exit

6. Abre una sesión en `nginx-sts-0` y dirijete hacia `nginx-sts-2`

       kubectl exec nginx-sts-0 -it -- /bin/sh
       curl http://nginx-sts-2.nginx-headless
       exit

7. Elimina el segundo pod.

       kubectl delete pod nginx-sts-2

8. ¿Sigue el archivo ahí?

    Abre una sesión en `nginx-sts-2` y verifica si el archivo sigue presente.

       kubectl exec nginx-sts-2 -it -- /bin/sh
       ls var/www
       exit

9. Limpiamos

       kubectl delete -f statefulset.yaml
       kubectl delete pvc www-nginx-sts-0
       kubectl delete pvc www-nginx-sts-1
       kubectl delete pvc www-nginx-sts-2

# Job

Un trabajo (Job) crea uno o más Pods y continuará reintentando la ejecución de los Pods hasta que un número específico de ellos finalice con éxito. A medida que los pods se completan con éxito, el trabajo realiza un seguimiento de las finalizaciones exitosas. Cuando se alcanza un número específico de finalizaciones exitosas, la tarea (es decir, el trabajo) se completa. Eliminar un trabajo limpiará los Pods que creó. La suspensión de un trabajo eliminará sus pods activos hasta que el trabajo se reanude nuevamente.

Un caso simple es crear un objeto Job para ejecutar de manera confiable un Pod hasta su finalización. El objeto Job iniciará un nuevo Pod si el primer Pod falla o se elimina (por ejemplo, debido a una falla del hardware del nodo o al reinicio del nodo).

1. Creamos nuestro trabajo.

   Creamos nuestro archivo YAML

      ```yaml
       apiVersion: batch/v1
       kind: Job
       metadata:
         name: hello
       spec:
         template:
           spec:
              containers:
              - name: busybox
                image: busybox
                command: ["echo", "Hello from the Job"]
              restartPolicy: Never
      ```

       kubectl apply -f job.yaml

2. Obtenemos la lista de nuestros trabajos.

       kubectl get jobs

3. Obtenemos mas información.

       kubectl describe job

4. Obtenemos el nombre del pod.

       kubectl get pods

5. Obtenemos los resultados del trabajo.

       kubectl logs <podName>

6. Limpiamos

       kubectl delete -f job.yaml

# CronJob

Un CronJob crea Jobs en un horario repetitivo.

CronJob está pensado para realizar acciones regulares programadas como copias de seguridad, generación de informes, etc. Un objeto CronJob es como una línea de un archivo crontab (tabla cron) en un sistema Unix. Ejecuta un trabajo periódicamente en un horario determinado, escrito en formato Cron.

1. Creamos un Job.

      ```yaml
       apiVersion: batch/v1
       kind: CronJob
       metadata:
         name: hello-cron
       spec:
         schedule: "* * * * *"
         jobTemplate:
           spec:
             template:
               spec:
                 containers:
                 - name: busybox
                 image: busybox
                 command: ["echo", "Hello from the CronJob"]
              restartPolicy: Never
      ```

       kubectl apply -f cronjob.yaml

2. Obtener la lista de los Jobs.

       kubectl get cronjobs

3. Describir los Jobs.

       kubectl describe cronjob

4. Obtenemos los nombres de los pods.

       kubectl get pods

5. Obtenemos los logs del contenedor.

       kubectl logs <podName>

6. Limpiamos

       kubectl delete -f cronjob.yaml

# Rolling Updates

1. Creamos un despliegue con la version v1.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-dep
  namespace: default
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  selector:
    matchLabels:
      app: hello-dep
  template:
    metadata:
      labels:
        app: hello-dep
    spec:
      containers:
      - image: guybarrette/hello-app:1.0
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi      
        imagePullPolicy: Always
        name: hello-dep
        ports:
        - containerPort: 8080
```
       kubectl create -f hello-deployment.yaml

2. Obtenemos el estado del despliegue.

       kubectl rollout status deployment/hello-dep

3. Obtenemos la lista de los pods.

       kubectl get pods -o wide

4. Describimos a nuestro pod

       kubectl describe pod hello-dep

5. ¿Cuantas replicas tenemos?

       kubectl get rs


### Creamos el segundo despliegue con la version v2.

Edite el archivo YAML y cambie la versión del contenedor de 1.0 a 2.0. Guarde el archivo.
 
1. Creamos nuestro despliegue.

       kubectl apply -f hello-deployment.yaml

2. Obtenemos el estado del despliegue.

       kubectl rollout status deployment/hello-dep

3. Obtenemos la lista de nuestros pods

       kubectl get pods -o wide

4. ¿Cuantas ReplicaSets tenemos?

       kubectl get rs

5. Obtenemos el historial de despliegues.

       kubectl rollout status deployment/hello-dep

---
 
### Rollback

1. Deshacemos los cambios del despliegue anterior.

       kubectl rollout undo deployment/hello-dep

    ó

       kubectl rollout undo deployment/hello-dep --to-revision 1

2. Obtenemos el historial de despliegues.

       kubectl rollout status deployment/hello-dep

3. ¿Cuantas ReplicaSets tenemos?

       kubectl get rs

4. Limpiamos

       kubectl delete -f hello-deployment.yaml

# Despliegue azul y verde

El despliegue azul y verde (Blue-Green Deployments) es un modelo de lanzamiento de aplicaciones que transfiere gradualmente el tráfico de usuarios de una versión anterior de una aplicación o microservicio a una nueva versión casi idéntica, ambas en producción. 

La versión anterior puede denominarse entorno azul, mientras que la nueva versión puede conocerse como entorno verde. Una vez que el tráfico de producción se ha transferido completamente del entorno azul al verde, el azul puede estar en espera en caso de retroceso o extraerse de producción y actualizarse para convertirse en la plantilla sobre la que se realiza la siguiente actualización.

1. Creamos el despliegue V1.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-v1
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  selector:
    matchLabels:
      app: hello-v1
  template:
    metadata:
      labels:
        app: hello-v1
    spec:
      containers:
      - image: guybarrette/hello-app:1.0
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi      
        imagePullPolicy: Always
        name: hello-v1
        ports:
        - containerPort: 8080
```

    kubectl create -f hello-dep-v1.yaml

2. Creamos el servicio ClusterIP

```yaml 
apiVersion: v1
kind: Service
metadata:
 name: svc-front
spec:
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    app: hello-v1
```

    kubectl create -f clusterip.yaml

3. Obtenemos la lista de los pods.

       kubectl get pods -o wide

4. Mostramos la aplicación en nuestro navegador.

   En primer lugar, reenvíe el puerto al ClusterIP:

       kubectl port-forward service/svc-front 8080:8080

   Abra un navegador y vaya a http://localhost:8080
   
   La versión de la aplicación será V1.

### Creamos el despliegue v2.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-v2
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  selector:
    matchLabels:
      app: hello-v2
  template:
    metadata:
      labels:
        app: hello-v2
    spec:
      containers:
      - image: guybarrette/hello-app:2.0
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi      
        imagePullPolicy: Always
        name: hello-v2
        ports:
        - containerPort: 8080
```

    kubectl create -f hello-dep-v2.yaml

1. Obtenemos la lista de nuestros pods.

    kubectl get pods -o wide

2. Editamos el manifiesto del ClusterIP.

   Edite el archivo clusterip.yaml y cambie la última línea para que el servicio apunte a nuestro despliegue V2.

       app: hello-v2

3. Actualizamos el servicio ClusterIP.

    kubectl apply -f clusterip.yaml

4. Mostramos la aplicación en el navegador.

   En primer lugar, reenvíe el puerto al ClusterIP:

       kubectl port-forward service/svc-front 8080:8080

   Abra un navegador y vaya a http://localhost:8080
   
   La versión de la aplicación será V2.

5. Limpiamos

       kubectl delete -f hello-dep-v1.yaml
       kubectl delete -f hello-dep-v2.yaml
       kubectl delete -f clusterip.yaml

# ClusterIP

ClusterIP es el tipo de servicio por defecto en Kubernetes, y proporciona conectividad interna entre los diferentes componentes de nuestra aplicación. Kubernetes asigna una dirección IP virtual a un servicio ClusterIP al que sólo se puede acceder desde dentro del clúster durante su creación. Esta dirección IP es estable y no cambia incluso si los pods detrás del servicio son reprogramados o reemplazados.

Los servicios ClusterIP son una excelente opción para la comunicación interna entre diferentes componentes de nuestra aplicación que no necesitan estar expuestos al mundo exterior. Por ejemplo, si tenemos un microservicio que procesa datos y los envía a otro microservicio para su posterior procesamiento, podemos utilizar un servicio ClusterIP para conectarlos.

### Desplegar el servicio.

```yaml
apiVersion: v1
kind: Service
metadata:
 name: svc-example
spec:
  ports:
  - port: 8080
    targetPort: 80
  selector:
    app: app-example
    env: prod
```

    kubectl apply -f clusterip.yaml

### Desplegamos nuestra app.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-example
spec:
  replicas: 3
  revisionHistoryLimit: 3
  selector:
    matchLabels:
      app: app-example
      env: prod
  template:
    metadata:
      labels:
        app: app-example
        env: prod
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

    kubectl apply -f deploy-app.yaml

### Desplegamos nuestra imagen de Busybox.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mybox
spec:
  restartPolicy: Always
  containers:
  - name: mybox
    image: busybox
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi    
    command:
      - sleep
      - "3600"
```

    kubectl apply -f pod.yaml

1. Obtenemos la lista de los pods.

       kubectl get pods -o wide

2. Nos conectamos a nuestro contenedor de BusyBox.

       kubectl exec mybox -it -- /bin/sh

3. Descargamos la pagina inicial de un servidor Nginx a travez del servicio de ClusterIP.

       wget -qO- http://svc-example:8080
       exit

4. Limpiamos

       kubectl delete -f clusterip.yaml
       kubectl delete -f deploy-app.yaml
       kubectl delete -f pod.yaml --grace-period=0 --force

# NodePort

Los servicios NodePort amplían la funcionalidad de los servicios ClusterIP permitiendo la conectividad externa a nuestra aplicación. Cuando creamos un servicio NodePort en cualquier nodo del clúster que cumpla los criterios definidos, Kubernetes abre un puerto designado que reenvía el tráfico al servicio ClusterIP correspondiente que se ejecuta en el nodo.

Estos servicios son ideales para aplicaciones que necesitan ser accesibles desde fuera del cluster, como aplicaciones web o APIs. Con los servicios NodePort, podemos acceder a nuestra aplicación utilizando la dirección IP del nodo y el número de puerto asignado al servicio.

### Desplegamos nuestra app.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-nginx
  labels:
    app: nginx
    env: prod
spec:
  replicas: 2
  revisionHistoryLimit: 3
  selector:
    matchLabels:
      app: nginx
      env: prod
  template:
    metadata:
      labels:
        app: nginx
        env: prod
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```
    kubectl apply -f deploy-app.yaml

### Desplegamos nuestro servicio NodePort.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-example
spec:
  type: NodePort
  selector:
    app: nginx
    env: prod
  ports:
  - nodePort: 32410
    protocol: TCP
    port: 80
    targetPort: 80
```

    kubectl apply -f nodeport.yaml

1. Obtenemos la lista de nuestros pods.

       kubectl get pods -o wide

2. Usamos nuestro nodeport

       kubectl get nodes -o wide

3. Limpiamos

       kubectl delete -f nodeport.yaml
       kubectl delete -f deploy-app.yaml

# LoadBalancer

Los servicios LoadBalancer conectan nuestras aplicaciones externamente, y los entornos de producción los utilizan cuando la alta disponibilidad y la escalabilidad son críticas. Cuando creamos un servicio LoadBalancer, Kubernetes aprovisiona un balanceador de carga en nuestro entorno de nube y reenvía el tráfico a los nodos que ejecutan el servicio.

Los servicios LoadBalancer son ideales para aplicaciones que necesitan gestionar grandes volúmenes de tráfico, como aplicaciones web o API. Con los servicios LoadBalancer, podemos acceder a nuestra aplicación utilizando una única dirección IP asignada al balanceador de carga.


### Desplegar nuestra app.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-nginx
  labels:
    app: nginx
    env: prod
spec:
  replicas: 2
  revisionHistoryLimit: 3
  selector:
    matchLabels:
      app: nginx
      env: prod
  template:
    metadata:
      labels:
        app: nginx
        env: prod
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

    kubectl apply -f deploy-app.yaml

### Desplegar nuestro servicio Load Balancer.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-example
spec:
  type: LoadBalancer
  selector:
    app: nginx
    env: prod
  ports:
  - protocol: TCP
    port: 8080
    targetPort: 80
```

    kubectl apply -f loadbalancer.yaml

1. Obtenemos la lista de nuestros pods.

       kubectl get pods -o wide

2. Usamos nuestro Load Balancer

       kubectl get svc -o wide

3. Limpiamos

       kubectl delete -f loadbalancer.yaml
       kubectl delete -f deploy-app.yaml

# Persistence Volumen

La gestión del almacenamiento es un problema distinto de la gestión de las instancias informáticas. El subsistema PersistentVolume proporciona una API para usuarios y administradores que abstrae los detalles de cómo se proporciona el almacenamiento de cómo se consume. Para ello, introducimos dos nuevos recursos API: PersistentVolume y PersistentVolumeClaim.

- Un PersistentVolume (PV) es una pieza de almacenamiento en el cluster que ha sido aprovisionada por un administrador o aprovisionada dinámicamente usando Clases de Almacenamiento. Es un recurso del cluster al igual que un nodo es un recurso del cluster. Los PVs son plugins de volumen como los Volumes, pero tienen un ciclo de vida independiente de cualquier Pod individual que utilice el PV. Este objeto API captura los detalles de la implementación del almacenamiento, ya sea NFS, iSCSI, o un sistema de almacenamiento específico del proveedor de la nube.

- Una PersistentVolumeClaim (PVC) es una solicitud de almacenamiento por parte de un usuario. Es similar a un Pod. Los Pods consumen recursos de nodos y los PVCs consumen recursos de PV. Los Pods pueden solicitar niveles específicos de recursos (CPU y Memoria). Los Claims pueden solicitar tamaños y modos de acceso específicos (por ejemplo, pueden ser montados ReadWriteOnce, ReadOnlyMany, ReadWriteMany, o ReadWriteOncePod, ver AccessModes).


### Cremos nuestro Persistent Volume.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv001
  labels:
    type: local
spec:
  storageClassName: ssd 
  capacity:
    storage: 10Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: "/data/"
```

    kubectl apply -f pv.yaml

1. Checamos nuestro pv.

       kubectl get pv

### Desplegamos nuestra demanda.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Mi
  storageClassName: ssd
```

    kubectl apply -f pvc.yaml

1. Checamos nuestro pvc

       kubectl get pvc

### Desplegamos nuestro pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mybox
spec:
  restartPolicy: Always
  containers:
  - name: mybox
    image: busybox
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi    
    command:
      - sleep
      - "3600"
    volumeMounts:
      - mountPath: "/demo/"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

    kubectl apply -f pod.yaml

1. Nos conectamos a nuestra instancia Busybox.

       kubectl exec mybox -it -- /bin/sh

2. Creamos un archivo.

       cd demo
       cat > hello.txt
       Hello World
       Enter and Ctrl-D
       ls
       exit

3. Eliminamos nuestro pod

       kubectl delete -f pod.yaml --force --grace-period=0

4. Desplegamos nuestro pod denuevo.

       kubectl apply -f pod.yaml

5. Nos conectamos a la instancia

       kubectl exec mybox -it -- /bin/sh
       cd demo
       ls
       cat hello.txt
       exit

6. Limpiamos

       kubectl delete -f pod.yaml  --force --grace-period=0
       kubectl delete -f pvc.yaml
       kubectl delete -f pv.yaml


# ConfigMap

Un ConfigMap es un objeto API utilizado para almacenar datos no confidenciales en pares clave-valor. Los pods pueden consumir ConfigMaps como variables de entorno, argumentos de línea de comandos o como archivos de configuración en un volumen.

Un ConfigMap te permite desacoplar la configuración específica del entorno de tus imágenes de contenedor, para que tus aplicaciones sean fácilmente portables.

### Creamos nuestro ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cm-example
data:
  state: Michigan
  city: Ann Arbor
```

    kubectl apply -f cm.yaml

1. Obtenemos detalles de nuestro ConfigMap.

       kubectl get cm
       kubectl describe configmap cm-example

   Imprimir la informacion en formato YAML

       kubectl get configmap cm-example -o YAML

### Desplegar nuestro pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mybox
spec:
  restartPolicy: Always
  containers:
  - name: mybox
    image: busybox
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi    
    command:
      - sleep
      - "3600"
    env:
      - name: CITY
        valueFrom:
          configMapKeyRef:
            name: cm-example
            key: city
```
    kubectl apply -f pod.yaml

1. Conectarnos a nuestro pod Busybox

       kubectl exec mybox -it -- /bin/sh

2. Mostrar la variable de entorno CITY
    
       echo $CITY
       exit

3. Limpiamos

       kubectl delete -f cm.yaml
       kubectl delete -f pod.yaml --grace-period=0 --force

# Secrets

Un Secreto es un objeto que contiene una pequeña cantidad de datos sensibles como una contraseña, un token o una clave. Esta información podría incluirse en una especificación Pod o en una imagen de contenedor. Utilizar un Secreto significa que no necesitas incluir datos confidenciales en el código de tu aplicación.

Dado que los Secrets pueden crearse independientemente de los Pods que los utilizan, hay menos riesgo de que el Secret (y sus datos) queden expuestos durante el flujo de trabajo de creación, visualización y edición de Pods. Kubernetes, y las aplicaciones que se ejecutan en su clúster, también pueden tomar precauciones adicionales con los Secretos, como evitar escribir datos sensibles en almacenamiento no volátil.

Los Secrets son similares a los ConfigMaps pero están específicamente pensados para contener datos confidenciales.

Para codificar/decodificar rápidamente cadenas en base64

**https://www.base64encode.org/**

**https://www.base64decode.org/**

en Linux/Mac

    echo [string] | base64
    echo [encodedString] | base64 -d

### Creamos nuestro Secrets

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secrets
type: Opaque
data:
  username: VGhlVXNlck5hbWU=
  password: bXlwYXNzd29yZA==
```

    kubectl apply -f secrets.yaml

1. Obtenemos información de nuestros secrets

       kubectl get secret
       kubectl describe secret secrets
       kubectl get secret secrets -o YAML

### Desplegamos nuestro pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mybox
spec:
  restartPolicy: Always
  containers:
  - name: mybox
    image: busybox
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi    
    command:
      - sleep
      - "3600"
    env:
      - name: USERNAME
        valueFrom:
          secretKeyRef:
            name: secrets
            key: username
      - name: PASSWORD
        valueFrom:
          secretKeyRef:
            name: secrets
            key: password  
```          

    kubectl apply -f pod.yaml

1. Conectamos a nuestra instancia Busybox

       kubectl exec mybox -it -- /bin/sh

2. Imprimimos nuestras variables de entorno USERNAME y PASSWORD.

       echo $USERNAME
       echo $PASSWORD
       exit

3. Limpiamos

       kubectl delete -f secrets.yaml
       kubectl delete -f pod.yaml --force --grace-period=0

# LivenessProbe

El kubelet utiliza sondas de liveness para saber cuándo reiniciar un contenedor. Por ejemplo, las sondas de liveness podrían detectar un punto muerto, en el que una aplicación se está ejecutando, pero no puede progresar. Reiniciar un contenedor en tal estado puede ayudar a que la aplicación esté más disponible a pesar de los errores.

Un patrón común para las sondas de liveness es utilizar el mismo endpoint HTTP de bajo coste que para las sondas de readiness, pero con un failureThreshold más alto. Esto asegura que el pod es observado como no preparado durante un periodo de tiempo antes de que sea eliminado.

### Desplegamos nuestro pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-example
spec:
  containers:
  - name: liveness
    image: busybox
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi    
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 15; rm -rf /tmp/healthy; sleep 3600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
      failureThreshold: 2
```

    kubectl apply -f pod.yaml

1. Miramos los eventos del nuestro pod.

       kubectl describe pod liveness-example

2. Limpiamos

       kubectl delete -f pod.yaml --force --grace-period=0