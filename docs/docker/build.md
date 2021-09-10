## Build

constuir inagen en base a la carpet donde se encuetra el dockerfile

``` 
docker build -t nombreimagen folder/
``` 

```
docker build -t nombreimagen:v1 .
``` 

usando dockerfile en otro directorio

``` 
docker build . -f ./path/to/dockerfile
``` 

``` 
docker build -t imagen . -f .docker/Dockerfile
``` 

ejecutar imagen 

``` 
docker run -it --rm nombreimagen 
``` 

``` 
docker run -it --rm -p 8080:8080 nombreimagen 
``` 