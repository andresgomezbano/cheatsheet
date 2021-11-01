# Deployments y Despliegues

## Workload y Controlles
Un workload es utilizado para poder desplegar contenedores. Un workload mas basico en un Pod, sin embargo este no tiene todas las caracteristicas (creacion de nuevas replicas, update, rollback) que se pueden hacer con otros objectos como los "deployments".

- Los "Deployments" envuelven a los "pods" y los dotan de mas caracteristicas como poder hacer updates y rollback de manera sencilla y comprobar el status.

- Los "Replica Set" son los encargados de hacer el escalado de los pods segun lo que el cluster tenga configurado.

- Los "Statefull Set" gestionan el despliegue y el escalado y garantiza el orden y unicidad de los pods.

- Los "Daemon Set" es un componente que todos los nodos de un cluster tengan una copida de un pod. Si se anada un nuevo nodo este componente trata de que el nodo tenga un Pod.

- Los "Jobs" es un componente que crea uno o varios "Pods" y se va a asegurar que un determinado # de ellos termine de forma correcta. Una vez que verifica que funcione de forma correcta el job terminal.

- El "Cron Job" es similar al Job pero se lo hace de forma planificada.

Todos estos conceptos estan muy relacionados, muchas de las caracteristicas son comunes. Siempre teniendo el trabajo de mantener el estado de los pods.

Todos estos componentes a su vez tienen asociados "Controllers", los controllers son componentes que estan consultando cada cierto tiempo el estado del cluster para mantenerlo segun lo deseado.

### Deployments
Los deploymnents es una especie de burbuja que engloba ciertas características  (entre ellas los pods), lo que me permite manejar rollbacks, updates, manejar escalamiento y tolerancia a fallos.

Cuando creo un deployment tb se crea un ReplicaSet, este es el encargado de hacer la replica y gestionar toda la recuperacion ante caidas.

Normalmente los replica sets se los maneja con un deployment pero si es posbile crearlos de forma separada.

# Modo Imperativo

Para crear un deployment de forma imperativa a partir de una imagen
```
kubectl create deployment apache --image=httpd
```

Esto nos crear el deployment, replicaset y los pods

```
kubectl get deploy
```

```
kubectl get rs
```

```
kubectl get pods
```

Describir las caracteristicas del deployment

```
kubectl describe deploy/apache
```

o tambien a traves del comando get podemos ver el estado deseado y el estado actual

```
kubectl get deploy apache -o yaml
```

# Modo declarativo

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-d
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 
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

```
kubectl apply -f nginx.yaml
```

Podemos ver lo que se ha creado filtrando por el label
```
kubectl get pods,rs,deploy -l app=nginx
```

## Editar Deployment

Podemos editar del deployment modificanco el yaml orginal y volviendo a aplicar el comando apply

```
kubectl apply -f nginx.yaml
```

otra opcion es editar directamente el archivo de configuracion por medio de kubectl

```
kubectl edit deploy nginx-d (nginx-d es el nombre del deploy)
```

## Escalar Replicas
Podemos hacerlo modificando el archivo de configuracion yaml o directamente desde kubectl

```
kubectl scale deploy nginx-d --replicas=5
```

o buscando por un label

```
kubectl scale deploy -l app=nginx --replicas=2
```

## Recursos
Para poder revisar los recursos con los que disponemos en el node hacemos uso del comando

```
kubectl describe node
```

### Deploy especificando cpu y memoria

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
  replicas: 5
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
        resources:
          limits:
              memory: "200M"
              cpu: "2"
          requests:
              memory: "100M" 
              cpu: "1"
```

Podemos especificar dentro de la configuracion del deploy la memoria y  cpu que queremos sea utilizada por cada pod.

Si los recursos no se han podido crear podemos verificar a traves de comando get pods los que esten como pendientes

```
kubectl get pods
```

y despues realizar un describe

```
kubectl describe pod nombredelpos
```

## Port Forwarding
Podemos conectarnos directamente hacia un pod haciendo port forward.

```
kubectl port-forward deployment-56b9cb4fb7-rf9bh 8080:80 
```