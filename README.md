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

### comprovacion:

desde el host, nos vamos loguear:
```
[local01@host docker]$ su - pere
Password: 
Creating directory '/tmp/home/pere'.
reenter password for pam_mount:
[pere@host ~]$ ll 
total 0
drwxr-xr-x. 2 pere users 0 Feb 21 12:32 pere
[pere@host ~]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          89G   19G   66G  23% /
tmpfs           1.9G     0  1.9G   0% /dev
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/sda5        89G   19G   66G  23% /etc/hosts
shm              64M     0   64M   0% /dev/shm
//samba/pere     89G   24G   66G  27% /tmp/home/pere/pere
```

#### otra comprovacion, desde fuera.

```
mount -t cifs -o "user=pere" //172.99.0.4/pere /mnt
```
> atencion! warning! yeeeee!!:
en este caso hemos utilizado la direccion ip 172.99.0.4 correspondiente al samba 
pero esto puede variar segun el orden de puesta en marcha de los containers

> Para evitarlo podemos editar el /etc/hosts y ejecutar:

```
mount -t cifs -o "user=pere" //samba/pere /mnt
```




