## Volumenes

Para compartir ficheros entre nuestro host y contenedores utilizamos los volúmenes.  Pueden ser compartidos entre todos los contedores que creemos

``` 
docker run -it -v /datos --name ubuntu1 ubuntu bash
```

Al especificar "-v /datos" crearemos un directorio compartido para nuestro contenedor dentro de "/var/lib/docker/volumes",  este volumen es persistente aún después de eliminar el contenedor.

Para listar los volumenes creados:

```
docker volume ls
```

```
docker volume inspect  <volumeid>, (puede ver el lugar de montaje)
```

Si queremos montar un directorio existente dentro del contenedor

```
docker run -it -v /home/andres/shared:/shared --name ubuntu2 ubuntu
```

### Compartir volumenes

docker run -it -v /datos --name ubuntu1 ubuntu bash

docker run -it --name ubuntu2 --volumes-from ubuntu1 ubuntu bash

docker run -it --name ubuntu3 --volumes-from ubuntu2 ubuntu bash

Si se eliminan todos los contenedores que estén utilizando el volumen, este ya no se lo podrá montar a uno nuevo, sin embargo los datos aun estarán en /var/lib/docker/volumes/<volumeid>

Para limpiar todos los volumenes que no sean utilizados ejecutamos:

docker prune

### Volumenes propios

docker volume create vol1

docker run -it --name ubuntu7 -v vol1:/dir1 ubuntu bash

docker run -it --name ubuntu8 -v vol1:/datos:ro ubuntu bash

:ro lo utilizamos para que se monte como sólo lectura

### Borrar volumenes

```
docker volume rm <volumeid>
```

```
docker volume prune (limpiar volumenes que no se estén utilizando por contenedores)
```