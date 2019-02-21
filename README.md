# practica_sambahomes
## roberto@edt asix m06 curs 2018-2019

### descricion:

> repositorio que contiene 3 containers:
* ldap
* hostpam
* samba

todos conectados en la misma red

### objetivo:
> conectividad entre los tres recursos, en los cuales se puedan loguear los usuarios y monte sus
directorios homes provenientes del recurso samba que tiene los shares de los homes de los usuarios.

### red:
> creamos una red nueva para no tener problemas en el momento de la ejecuccion de los containers

#### execucio

creamos la red en la cual vamos a conectar los recursos
``` 
docker network create --subnet 172.99.0.0/16 --gateway 172.99.0.1 sambanet
```

arrancamos la imagenes pertinentes 

```
docker run --rm --name ldap -h ldap --network sambanet -d robert72004/ldapserver_18roberto
docker run --rm --name host -h host --privileged --network sambanet -it robert72004/hostpam:18sambahomes
docker run --rm --name samba -h samba --network sambanet -it robert72004/samba:18homes
```




