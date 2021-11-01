#Services
Los servicios son uno de los componentes mas importantes porque nos permiten conectarnos de forma externa hacia nuestro pods. Funciona como una capap entre el cliente y los deployments.

- ClusterIp
Para uso interno dentro del cluster. Por Ej.: Bases de datos
Nos da: ip, nombre y puerto

- NodePort
Para uso externo. Por Ej.: Aplicacion Web
Nos da: ip, nombre, puerto y nodeport

- LoadBalander
Para uso externo pero integrado con otros loadbalancers de terceros como:  Google, Amazon o Azure.

El servicio busca los objetos por los labels y segun esto los agrega a su grupo.

## NodePort Declarativo

Podemos exponer un deployment al exterior por medio de un servicio de tipo nodeport
```
kubectl expose deployment web-d --name=web-svc --target-port=80 --type=NodePort
```

Verificamos los servicios y el puerto que nos asigno kubernetes

```
kubectl get svc
```
o por medio de 

```
 kubectl describe svc web-svc
```

Podemos verifica la ip del cluster por medio de:

```
minikube ip
```
o

```
kubectl get node -o wide
```
## Nodeport Imperativo

```
apiVersion: v1
kind: Service
metadata:
  name: web-service
  labels:
     app: web
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30002
    protocol: TCP
  selector:
     app: web
```

Podemos crear el servicio de forma imperativa sobre los recursos que hayamos creado previamente (en un depoyment por ej.) con un label especifico. Para este caso el label 'web'.

```
kubectl apply -f service.yaml
```


## Roll Out Updates
Cuando hacemos un cambio en la plantilla de los contenedores kubernetes los va a destruir o crear hasta llegar al estado deseado. Tenemos algunas opciones para definir este comportamiento:

-Roll out
Nos permitira recrear poco a poco cada pod con el fin de que el servicio no se corte.

-Recreate
Volvera a crear todos los pods lo que nos dejara sin servicio hasta que temrmine la actualizacion

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-d
  labels:
    estado: "1"
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 10
  strategy:
     type: RollingUpdate
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

Si aplicamos un deploy y luego modificamos su imagen es seguro que tenga que volver a reconstruir todos lod pods

```
    spec:
      containers:
      - name: nginx
        image: nginx:1.17.8
        ports:
        - containerPort: 80
```


```
kubectl apply -f deploy.yaml
```

Podemos ver el historial de rollouts que se han realizado sobre ese deploy

```
kubectl rollout history deploy nginx-d
```

Tambien podemos visualizar en mayor detalle cada rollout y el hash template que se le asigna a los pods
```
kubectl rollout history deploy nginx-d --revision=1
```

Si revisamos los pods mientras se esta realizando el roll out podemos visualizar en cada pod el hash template actual y como van cambiando de a poco al nuevo segun se van actualizando los pod.

```
Kubectl get pods
```
Podemos visualizar la configuracion del replica set, para el nuevo roll out se creara una nueva configuracion

```
kubectl get rs
```

```
  strategy:
     type: RollingUpdate
     rollingUpdate:
        maxUnavailable: 1
        maxSurge: 1
  minReadySeconds: 2
```

Podemos definir ciertos parametros para configurar nuestra actualizacion. por ej.:

- maxUnavaialble: cantidad maxima de pods que se pueden encontrar inactivos en el proceso de actualización
- maxSurge: cantidad máxima de pods que  pueden ser aumentados a nuestras replicas para poder realizar el proceso de actualización 
- minReadySeconds: tiempo mínimo que debe pasar para que nuestros pods se consideren activos.

## Roll Back

Si nuestro deploy está fallando podemos regresar a un deploy anterior especificando la revision:

```
kubectl rollout undo deployment nginx-d --to-revision=2
```

Podemos ver siempre el estado del deploy

```
kubectl rollout status deploy nginx-d
```

y revisar los pods

```
kubectl get pods
```

o replica sets:

```
kubectl get rs
```

## Recreate

```
  strategy:
     type: RollingUpdate
```

La otra estrategia es recreate, en esta se eliminar todos los pods al mismo tiempo y se los vuelve a crear. Muy util para casos donde la aplicación esta caida y se necesita modificarla rápidamente.

## Ver yaml recurso
Podemos ver el yaml del recurso que se cree por medio del comando

```
kubectl get svc service -oyaml
```