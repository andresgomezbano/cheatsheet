# Minikube

## Comandos Iniciales

Poder ver el estado de minikube

```
minikube status
```

Para iniciar minikube

```
minikube start
```


[https://github.com/kubernetes/minikube/releases]


```
minikube stop
chmod +x minkube-file
mv minkube-file /usr/local/bin/minkube
minkube version
```
## Otros comandos
Para visualizar un dashboard con informacion del cluster
```
minikube dashboard
```

Saber que direccion se le ha asignado al cluster
```
minikube ip
```

Ver los los del sistema
```
minikube log
```

Para ingresar a la maquina/contenedor donde se esta ejecutando minikube
```
minikube ssh
```

Para verficar si hay actualizaciones disponibles
```
minikube check-version
```

## Iniciar Minikube con otro Container Runtine
[https://minikube.sigs.k8s.io/docs/handbook/config/#runtime-configuration]

```
minikube stop
minikube start --container-runtime=cri-o
```

Para ver informacion de los nodos y su container runtine

```
minikube start --container-runtime=docker
```

## Profiles
Podemos crear varios profiles en minikube haciendo

```
minikube start -p desarrollo
```

Para ver el profe actual

```
minikube profile
```

Listar Profiles

```
minikube profile list
```

Cambiar de profile

```
minikube profile profilededestino
```

Al ejecutar comandos de minikube afectamos al profile actual por ej.

```
minikube stop
```

detendra unicamente el profile en el que nos encontramos.

Tambien podemos especificar el profile que queramos con el flag -p.

```
minikube delete -p desarrollo
```

Eliminara el profile en el que nos encontramos y liberara los recursos que estaba utilizando.


## Ficheros minikube
Podemos ver informacion del cluster de kubernetes, contextos, ips, current-context.

```
cd ~/.kube/
cat config
```

Para ver informacion sobre minikube

```
cd ~/.minikube/
cd machines
```

podemos ver una carpeta por cada cluster que se ha ejecutado