# VoT

## Instalación de Bases de Datos
### Correr PostgreSQL con Docker (Recomendado)

Ejecutar contenedor de postgreSQL
```shell
docker run --name some-postgres -e POSTGRES_USER=havasmediaRDS -e POSTGRES_PASSWORD=FfHGdiGU -e POSTGRES_DB=havasmediaDB -d -p 5432:5432 -d postgres
```

### Correr Elasticsearch con Docker (Recomendado)
~~~shell
docker run -d -p 9200:9200 \
-e "http.host=0.0.0.0" \
-e "transport.host=127.0.0.1" \
-e "xpack.security.enabled=false" \
-e "network.bind_host=0.0.0.0" \
docker.elastic.co/elasticsearch/elasticsearch:6.2.2
~~~

### Instalar PostgreSQL en Fedora 28
~~~
sudo dnf install https://download.postgresql.org/pub/repos/yum/10/fedora/fedora-28-x86_64/pgdg-fedora10-10-4.noarch.rpm
sudo dnf install postgresql10
sudo dnf install postgresql10-server
sudo /usr/pgsql-10/bin/postgresql-10-setup initdb
~~~
Configurar PostgreSQL
~~~
sudo postgresql-setup --initdb
sudo passwd postgres
su - postgres
createuser havasmediaRDS -P
createdb --owner=havasmediaRDS havasmediaDB
psql -h localhost -U havasmediaRDS -d havasmediaDB
~~~
Abrir el archivo `/var/lib/pgsql/data/pg_hba.conf`
buscar al final del archivo las siguientes lineas y remplazar la **ident** por **md5**:
~~~
host    all             all             127.0.0.1/32            ident
host    all             all             ::1/128                 ident
~~~
reiniciar PostgreSQL
~~~
sudo systemctl restart postgresql-10
~~~
## Instalar herramientas de desarrollo

### Fedora
```shell
sudo dnf install -y nodejs
sudo dnf install -y npm
sudo dnf install -y git
```
### Ubuntu
```shell
sudo apt-get install -y nodejs
sudo apt-get install -y npm
sudo apt-get install -y git
```
## Instalar sails
```shell
npm install sails -g
```
## Obtener el codigo del VoT
```
git clone https://github.com/havasMX/HavasMediaFrontend.git
git clone https://github.com/havasMX/HavasMediaBackend.git
```
## Instalar dependencias en Backend  
```
cd HavasMediaBackend
npm install
```
## Cambiar la url de AWS a localhost
En el archivo HavasMediaBackend/config/connections.js
remplazar la linea:
```javascript
 host: 'havasmediadb.c78u898gdi9y.us-west-2.rds.amazonaws.com',
 ```
por:
```javascript
host: 'localhost',
```
## Ejecutar backend
```shell
sails lift
```


## Cambiar la url de Elasticsearch a localhost
Hacer **find and replace in files** con un editor
buscar la url:
`https://search-havasmedia-h7o3s6bmcgvqv6ubgvkamqmiwa.us-west-2.es.amazonaws.com` y remplazar por: `localhost:9200`

## Cambiar la url de S3 a otro bucket copia
1. Crear un bucket copia
2. Crear un identify pool id https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html
3. Hacer **find and replace in files** con un editor
buscar la url generada en paso 2:
`us-west-2:97a951e7-b60c-4068-92e9-e6505f213116` y remplazar por: `nueva url`

## Instalar consola de angular
```
npm install -g @angular/cli
```
## Instalar dependencias fronted
```
cd HavasMediaFrontend
npm install
```
## Ejecutar fronted
```
ng serve
```

## Información addicional
puertos por servicio

| servicio      | puerto |
| ------------- | ---: |
| Elasticsearch | 9200 |
| PostgreSQL    | 5432 |
| Sails (NodeJS)| 1337 |
| Angular       | 4200 |
