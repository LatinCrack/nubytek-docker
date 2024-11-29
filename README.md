![Nubytek](https://github.com/LatinCrack/nubytek-docker/blob/main/nubytek.jpg)
### 1 Primer contenedor - Servidor Web Apache
#### Comando:
    $ docker run --name latinserver_web -d -p 82:80 httpd
    
| Option                  | Description                                                              |
| ----------------------- | -------------------------------------------------------------------------|
| `docker run`            | Ejecuta un contenedor en base a una imagen (httpd en este caso)          |
| `--name latinserver_web`| Permite darle un nombre personalizado al contenedor                      |
| `-d`                    | detached mode, Ejecuta el contenedor en segundo plano                    |
| `-p 82:80`              | Especifica el puerto, siendo 82 el puerto del host y 80 el del contenedor|
| `httpd`                 | Nombre de la imagen que se va a descargar desde el Docker Hub            |
***
### [2 Primer Dockerfile](2PrimerDockerFile) - Construyendo la Primera imágen
#### Previo: Dentro de la carpeta, donde se va a guardar el Dockerfile, deben estar los archivos (propias del Front):
- [x] index.html
- [x] prepros-6.config
- [x] prepros.config
#### Y las carpetas (propias del Front):
- [x] assets
- [x] vendor
#### Contenido del Dockerfile:
```
FROM httpd:latest

LABEL version="Web de LatinCrack"

COPY assets /usr/local/apache2/htdocs/assets
COPY vendor /usr/local/apache2/htdocs/vendor
COPY prepros.config /usr/local/apache2/htdocs/
COPY prepros-6.config /usr/local/apache2/htdocs/
COPY index.html /usr/local/apache2/htdocs/
```
#### Construyendo la imagen (miweb:ver1) con el Dockerfile:
`docker build -t miweb:ver1 .`
#### Ejecutar contenedor desde la imagen previamente creada (miweb:ver1):
`docker run --name weblatin -d -p 80:80 miweb:ver1`
***
### 3 Creando Repo en DockerHub y subir imagen
#### Entrar al DockerHub y crear un nuevo repositorio:

