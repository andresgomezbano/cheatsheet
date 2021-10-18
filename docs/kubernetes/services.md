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

## NodePort

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
