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
![image](https://github.com/user-attachments/assets/6dc33aa8-9f5d-4a87-a89b-699facb6275c)
Seleccionamos el namespace, le ponemos un nombre y lo creamos:
![image](https://github.com/user-attachments/assets/17f2707c-2c82-41c6-af35-fb45d2cdc6b0)
Ahora en local, donde tenemos la pagina web, vamos a crear una imagen con el user de dockerhub (latincrack/nubytek_latinweb:):

    $ docker build -t latincrack/nubytek_latinweb:ver1 .
Listamos las imagenes creadas:

![image](https://github.com/user-attachments/assets/ad867ee1-db0b-4a18-bfc4-c1f7f6ffa54b)

Ahora enviamos la imagen (PUSH) construida al Docker Hub (previa autentificacipon):

    $ docker push latincrack/nubytek_latinweb:ver1
![image](https://github.com/user-attachments/assets/f0585c4d-ffce-41e2-bd42-52c0d3ec3ff0)

Y luego en el Docker Hub podemos ver que ya se subio la imagen:

![image](https://github.com/user-attachments/assets/13481226-8261-44e1-8cbc-93fe6457bdc3)

***
### 4 Construyendo un contenedor en base al repo del dockerhub:
    $ docker run --name weblatin -p 80:80 -d latincrack/nubytek_latinweb:ver1
![image](https://github.com/user-attachments/assets/cd3257dd-76d0-46af-9646-f137ba292c63)
![image](https://github.com/user-attachments/assets/7019e90c-b867-4750-bc15-b74b22c5d6fb)

***
### 5 Sobreescribiendo la imagen del dockerhub (tener el index.html ya modificado):
#### El contenido del Dockerfile:
```
FROM latincrack/nubytek_latinweb:ver1
COPY index.html /usr/local/apache2/htdocs/index.html/
```
#### Comando para crear la imagen sobreescrita:
`docker build -t sobreescrito .`

#### Comando para crear el contenedor con la nueva imagen:
`docker run --name otraweb -d -p 81:80 sobreescrito`
![image](https://github.com/user-attachments/assets/a9c86a72-5872-4f92-8fe2-b353012262b8)

***






