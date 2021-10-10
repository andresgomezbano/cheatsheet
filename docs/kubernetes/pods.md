# Pods

Tengo dos formas de hacer un deploy hacia kubernetes:

- Imperativo
- Declarativo

Imperativo le digo por comandos despliega tal imagen con n replicas. Por ej. usando Kubectl run.

Declarativo utiliza un archivo de configuracion para hacer los deploys por medio de un yaml o json.

Cuando se hace un deploy hacia el cluster, este se almacena en un pod. El pod envuelve un container y le aporta recursos (Memoria, CPU, etc.).

Por lo general un pod tiene un solo contenedor, pero podria tener mas de uno. Depende del escalamiento por contenedor que les quiera dar.

Por ejemplo puede tener en el mismo Pod:

- backend y
- mongo

en ese caso cuando escale la aplicacion voy a tener n instancias de los 2 contenedores, sin embarlo lo que me conviene es tenerlos en pods separados y manejar el escalamiento individual.

Asi voy a tener recursos (cpu, memoria, hostnames, volumenes, ip) por cada Pod. 

Dentro de los pods no se debe almacenar nada, no deben tener estado, ya que se construyen y destruyen segun configuraciones. Para manejar persistencia debemos utilizar volumenes u otras herramientas.

## Ejecutar una imagen

Utilizamos el comando run, indicando el nombre que le queremos dar al pod y la imagen que queremos utilizar.


```
kubectl run nginx1 --image=nginx
```

Esta imagen es recuperada desde algun repositorio que este configurado, por defecto en dockerhub.

Para obtener el listado de pods ejecutandose utilizamos:

```
kubectl run nginx1 --image=nginx
```

o

```
kubectl run nginx1 --image=nginx
```

para ver mas informacion como la ip y el nodo del cluster donde se esta ejecutando.


## Describe
Con el comando kubectl describe se pude detallar las caracteriscas de cualquier componente.

Debo especificar el tipo de componente al que me refiero por ejemplo:

```
kubectl describe pod/nginx1
```

Nos da detalle del pod llamado nginx1


## Exec

Similar a docker nos permite entrar y ejecutar comandos dentro de un contenedor. Por ej.:

```
kubectl exec nombrecontenedor -- ls
```

Para ingresar en modo interactivo utilizamos el parametro -it

```
kubectl exec nombrecontenedor -it -- bash
```
## Logs

Si deseamos ver los logs del contenedor lo podemos hacer a travez del comando log

```
kubectl logs nginx1
```

Si queremos verlo en modo stream

```
kubectl logs -f nginx1
```

Podemos ver tb los ultimos n registros con tail

```
kubectl logs apache --tail 30
```

## Proxy

Se puede obtener informacion del cluster por medio del api de proxy, ejecutando el comando

```
kubectl proxy
```

Accedemos en el navegado al puerto 8001 y podemos navegar entre toda la info del cluster.

Por ejemplo para acceder a la info de un pod:

http://localhost:8001/api/v1/namespaces/default/pods/nginx1

Si quieremos entrar a ese contenedor por http lo podemos hacer utilizando:

http://localhost:8001/api/v1/namespaces/default/pods/nginx1/proxy

## Dentro del nodo

Dentro del nodo     del cluster podemos ver los contenedores que estan en ejecucion. Por ej. Utiliznado minikube accedmos a 

```
minikube ssh
```
y ejecutamos el comando 

```
docker ps
``` 
para visualizar informacion de contenedores en ejecucion, no es recomendable modificar el estado del cluster desde dentro del nodo ya que podemos dejarlo en un estado incorrecto.

## Crear Pod desde un Yaml
Podemos especificar las configuraciones para desplegar un pod por medio de un yaml.
```
kubectl create -f nginx.yaml
```

Para ver sus logs:
```
Kubectl logs nginx
```

Para ver las caracteristicas del pod:
```
Kubectl describe pods/nginx1
```
## Obtener un Pod
```
kubectl get pods
```

Podemos ver informacion mas detallada:

```
kubectl get pods -o wide
```

podemos especificar el formato

```
kubectl get pod/nginx2 -o yaml
```

## Borrar un Pod

```
kubectl get pods
```

```
kubectl delete pod/nginx1
```

```
kubectl delete pod nginx1, nginx2, nginx3
```

Especificar un perido de gracia:
```
kubectl delete pod apache --grade-period=5
```

Borrar los pods sin esperar a que se detengan:
```
kubectl delete pod apache --now
```

Borrat tods los pods
```
kubectl delete pods --all
```

## Comando Apply
Es un comando de tipo declarativo, nos permite crear o modificar un objeto mediante un archivo de configuracion (depediendo de su estado).

```
kubectl apply -f nginx.yaml
```


