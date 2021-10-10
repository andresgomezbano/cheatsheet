# Labels
Son simplemente etiquetas que se le ponen a los objetos. A traves de los labes se realizan  relaciones entre los distintos tipos de objetos.
Los agregamos nosotros segun lo necesitemos.

```
apiVersion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    staging: "desarrollo"
spec:
  containers:
   - name: tomcat
     image: tomcat
```

## Forma declarativa
Modificando el yaml y por medio del comando apply podemos modificar el valor de las etiquetas de manera declarativa.

```
kubectl apply -f file.yaml
```

## Forma imperativa

Podemos visualizar las etiquetas:
```
kubectl get pod/tomcat --show-labels
```

Podemos visualizar las etiquetas como una nueva columna:

```
kubectl get pod/tomcat --show-labels -L estado
```

Setear un nuevo label
```
kubectl label pod tomcat owner=Andres
```

Sobreescribir valor de un label

```
kubectl label pod/tomcat --overwrite estado=qa
```

Borrar un label:
```
kubectl label pod/tomcat estado-
```

## Selectores
Nos permite buscar objetod dentro del cluster por sus etiquetas.

```
kubectl get pods --show-labels -l staging=desarrollo
```

```
kubectl get pods --show-labels -l staging=desarrollo,owner=andres
```

```
kubectl get pods --show-labels -l staging!=desarrollo
```

```
kubectl get pods --show-labels -l 'staging in(desarrollo)'
```

```
kubectl get pods --show-labels -l 'staging notin(desarrollo)'
```

Podemos ejecutar acciones contra estos objetos por ej.:

```
kubectl delete pods -l staging=desarrollo
```