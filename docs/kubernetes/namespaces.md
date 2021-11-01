# Namespaces
Son espacios de nombre para agrupar ojetos. Es un nombre lógico que se le pone a una partición de kubernetes para tener de forma separada objetos dentro del cluster.

Puedo tener varios namespaces y dentro de ellos crear objetos para trabajar con ellos. Son una espacie de particiones virtuales.

Podemos ver los namespaces con el comando:

```
kubectl get namespace
```

Por defecto todo se crea en default si es que no se especifica lo contrario.

Kube-system es un namespace propio del cluster que contiene todos los objetos que se crean durante la instalación y es usado internamente por kubernetes.

kube-public es un namespace que se crea de forma automática y puede ser usado por todos los usuarios. Es usado internamente por el cluster pero tb puede ser utilizado para dejar objectos comunes entre todos los namespaces.

Puedo ver información sobre un namespace:

```
kubectl get namespace default
```

```
kubectl describe namespace default
```

Con la opción -n podemos especificar el namespace del que queremos ver los objetos.

```
kubectl get pods -n kube-system
```

## Crear Namespaces

### Imperativa
Podemos crear un namespace de forma imperativa

```
kubectl create namespace nuevo1
```

```
kubectl describe namespace nuevo1
```

Para eliminarlo:

```
kubectl delete namespace nuevo1
```

### Declarativa
```
apiVersion: v1
kind: Namespace
metadata:
  name: nuevo1
  labels:
     tipo: desarrollo
```

```
kubectl apply -f file.yaml
```

## Deployar en un namespace

```
kubectl apply -f deploy.yaml --namespace=nuevo1
```

```
kubectl get deploy name -n nuevo1
```

## Namespace por defecto

```
kubectl config set-context --current --namespace=nuevo1
```

## Limites a un namespace

```
apiVersion: v1
kind: LimitRange
metadata:
  name: recursos
spec:
  limits:
  - default:
      memory: 512Mi
      cpu: 1
    defaultRequest:
      memory: 256Mi
      cpu: 0.5
    max:
      memory: 1Gi
      cpu: 4
    min:
      memory: 128Mi
      cpu: 0.5
    type: Container
```

Podemos crear un objeto de tipo LimitRange, esto limitará los recursos del namespace.

```
kubectl apply -f limites.yaml -n nuevo1
```

