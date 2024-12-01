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
![image](https://github.com/user-attachments/assets/6354ab32-5075-43a5-92c6-9e070180471c)


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
![image](https://github.com/user-attachments/assets/4a03a063-83b8-4dd1-b927-3a7765b77204)


***
### 6 Redes en Docker:
#### Docker usa los siguientes drivers de redes:
![image](https://github.com/user-attachments/assets/234606c6-7773-4093-a916-67446ec9a4dd)

#### BRIDGE: Modo predeterminado, sino especificamos el parametro --network al ejecutar el contenedor, tomara este driver.
Este modo crea una interfaz de red virtual (su propia red), asignandole IP interna a cada contenedor.

Creamos un contenedor apache:

`docker run --name latinapache -p 85:80 -d httpd`

Y vemos los detalles de la red con:

`docker inspect latinapache`

![image](https://github.com/user-attachments/assets/e9b937be-dfdc-4598-961e-00c71c4bd861)

#### HOST: No crea una red aparte y usa directamente la IP del host.

`docker run --name latinapache -d --network host httpd`

![image](https://github.com/user-attachments/assets/a6b8ec8a-47a5-4fd5-94de-2ea7f8b9a264)

#### NONE: Crea un contenedor completamente aislado.
Ideal para contenedores que solo procesen datos, generar informes, o cualquier otro escenario donde no necesite comunicación ni conectividad de red.

`docker run --name latinapache -d --network none httpd`

![image](https://github.com/user-attachments/assets/51b397e0-c78a-4d45-b77c-eeb02ba90e64)

#### __host.docker.internal__ : es un nombre especial que resuelve a la dirección IP de la máquina host desde dentro de un contenedor Docker.

NOTA.- Esto es una caracteristica de Docker Desktop. En servidores Linux no está esta caracteristica.

***

#### DOCKER CP: Comando para copiar archivos en contenedores (usar: "docker cp -r" para copia recursiva).
#### Desde el host al contenedor:
`docker cp <archivo_local> <contenedor>:<ruta_dentro_del_contenedor>`
`docker cp index.html my_web_app:/var/www/html/`

#### Desde el contenedor al host:
`docker cp <contenedor>:<ruta_dentro_del_contenedor> <archivo_local>`
`docker cp my_container:/var/log/myapp.log /home/logs/`

***
### 7 Almacenamiento en Docker:
![image](https://github.com/user-attachments/assets/25bb2b01-b853-4c26-b5b9-f764145ab0f3)



