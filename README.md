# Servicio de lectura de fuentes sobre docker Tiny Tiny Rss

Contenedor docker con tiny tiny rss en su ultima version sobre alpine.

Este contenedor se basa en una imagen descargada de:

<https://hub.docker.com/r/x86dev/docker-ttrss>

Primero hay que crear un directorio de trabajo:

```bash
mkdir /opt/docker/
cd /opt/docker
```

## Crear la imagen de Alpine para tiny

No es necesaria crear la imagen, ya que el repositorio de docker esta subida.  

Descargar los archivos necesario de gitlab:

```bash
git clone https://github.com/cabedev/tinyrss
```

Creamos los diectorios de trabajo para los volumenes:

```bash
mkdir /opt/docker/tinyrss/volumes
```

Creamos el contenedor:

```bash
cd /opt/docker/tinyrss/image
docker build -t x86dev/docker-ttrss .
```

## Crear el contenedor de tiny

Creamos los contenedores:

Previamente se puede editar el fichero "/opt/docker/tinyrss/docker-compose.yml"
para cambiar la clave de la base de datos "cla2019ve"

```bash
cd /opt/docker/tinyrss/
docker-compose up -d
```

Creamos la base de datos y usuario anteriores desde phpmyadmin, accediendo con el usuario root y la clave "cla2019ve":

<http://localhost:8085/>

La base de datos la creamos con el cotejamiento "utf8_general_ci". Al crear el usuario le damos todos los permisos para esa base de datos.

Ronbramos el archivo de configuracion de tiny:

```bash
mv /opt/docker/tinyrss/volumes/www/ttrss/config.php /opt/docker/tinyrss/volumes/www/ttrss/config.php_old
```

Entramos en el navegador para comprobar que se carga el inicio de la instalacion de tiny:

<http://localhost:8080/install>

Lo instalamos especificando los datos según la base de datos y usarios creados en phpmyadmin, a modo de ejemplo:
- Database type: "MySQL"
- Username: "ttrssus" (usuario creado en phpmyadmin con permisos sobre la base de datos)
- Password: "ttrsspw" (clave del usuario anterior)
- Database name: "ttrssdb" (nombre de la base de datos creada en phpmyadmin)
- Host name: "mysql" (nombre del servidor de base de datos segun docker-compose.yml)
- Port: "3306" (puerto de la base de datos segun docker-compose.yml)

Seguimos la indicaciones de "Test configuration", "Initialize database" de la base de datos y creación del fichero de configuración con "Save configuration".

## Acceso a Tiny Tiny Rss

<http://localhost:8080/>
Usuario: admin
Clave: password

