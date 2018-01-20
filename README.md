# Instalación Postgres

Este documento contiene instrucciones para instalar, configurar y ejecutar el contenedor para Postgres.

No pongo un Dockerfile porque para instalar la BD se utiliza la imagen oficial de Postgres sin modificaciones, pero si hay que configurarlo y pasarle parametros al momento de ejecutar los comandos, asi como tambien ejecutar algunos codigos SQL desde la consola.

Ejecutar estos comandos para instalar la base de datos completamente.

En los siguientes comandos, se debe tener cuidado de reemplazar correctamente las strings **postgrespassword123456789** (clave global de postgres), **dockerdb** (nombre de la base de datos para la aplicacion), **dockeruser** (nombre de usuario), **dockerpass** (clave del usuario) por los valores que se desee.

Ejecutar (y descargar) la imagen de Postgres.

```bash
docker run --name local-postgres9.3.6 -p 5432:5432 -e POSTGRES_PASSWORD=postgrespassword123456789 -d postgres:9.3.6
```

Ingresar a la consola de Postgres para configurarla.

```bash
docker run -it --link local-postgres9.3.6:postgres --rm postgres:9.3.6 sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres'
```


Ejecutar lo siguiente en la consola de Postgres (la que comienza con `postgres-#`).

```sql
CREATE DATABASE dockerdb;
CREATE USER dockeruser;
alter user dockeruser with encrypted password 'dockerpass';
grant all privileges on database dockerdb to dockeruser;
\q
```



# En caso que haya conflicto de puertos

En caso que el puerto 5432 (default de Postgres) este ocupado en la maquina real donde se ejecuta el container, se puede mapear los puertos de manera distinta usando algo como (comando de ejemplo):


```bash
# Here port 80 of the host is mapped to port 5000 of the container
$ docker run -d -p 80:5000 training/webapp python app.py

```

Fuente: https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/#connect-using-network-port-mapping

# Probar conexion

Se puede probar si funciona con `telnet <IP o HOST> 5432` y hay que dejar abierto el puerto hacia el exterior o la red local donde este la aplicacion de Rails.

