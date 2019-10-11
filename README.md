# distributed-databases

A continuación se presentan las instrucciones para realizar un Demo de una base de datos distribuida (Replica set) con MongoDB.

Se asume que [mongoDB](https://www.mongodb.com/) se encuentra previamente instalado.

1. Crear una carpeta para el proyecto, en un destino accesible y donde el usuario tenga permisos.
2. Conectarse a la misma red que sus compañeros de demostración o realizarlo de forma local.
3. Determinar la dirección IP de su computador.

Este se puede obtener mediante el comando:
~~~
ipconfig
~~~
o mediante:

~~~
ifconfig
~~~

Según su sistema operativo.


4. Ejecutar el siguiente comando

~~~
mongod --replSet <nombre de cluster>  --bind_ip localhost,<direccion ip> --dbpath <direccion de la carpeta> 
~~~

Por ejemplo:
~~~
mongod --replSet rs0  --bind_ip localhost,172.18.144.18 --dbpath mongo0
~~~

5. Realizar la configuración en la base de datos que será considerada como primaria

Para esto desde mongo ejecute el siguiente camando:


~~~
rsconf = {
  _id: "<nombre de cluster>",
  members: [
    {
     _id: 0,
     host: "<hostname>:27017"
    },
    {
     _id: 1,
     host: "<hostname>:27017"
    },
    {
     _id: 2,
     host: "<hostname>:27017"
    }
   ]
}
~~~

Por ejemplo:

~~~
rsconf = {
  _id: "rs0",
  members: [
    {
     _id: 0,
     host: "172.18.144.18:27017"
    },
    {
     _id: 1,
     host: "172.18.144.22:27017"
    },
    {
     _id: 2,
     host: "172.18.144.19:27018"
    }
   ]
}
~~~

Seguido del siguiente comando:

~~~
rs.initiate(rsconf)
~~~

El estado del cluster se puede verificar mediante el comando

~~~
rs.status()
~~~

A partir de este punto el sistema se comporta como una base de datos tradicional.

Desde las bases de datos secundarias se pueden realizar consultas de busqueda, para ello deben primero asignarse como esclavos, mediante el comando:

~~~
rs.slaveOk();
~~~

Y posteriormente utilizar la base de datos correcta.


Los demás comandos disponibles pueden consultarse mediante:
~~~
rs.help()
~~~
