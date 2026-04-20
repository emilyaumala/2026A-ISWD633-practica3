## VOLUMEN NOMBRADO
Un volumen nombrado (named volume) es un tipo de volumen gestionado por Docker que se almacena en una ubicación específica del sistema de archivos del host y se identifica mediante un nombre único. Los volúmenes nombrados no requieren que especifiques una ruta del sistema de archivos del host, y en su lugar, Docker se encarga de la gestión y el almacenamiento del volumen.


### Crear volumen
```
docker volume create <nombre volumen>
```

### Crear el volumen nombrado: vol-postgres
# COMPLETAR CON EL COMANDO
docker volume create vol-postgres

## MOUNTPOINT
Un mountpoint se refiere al lugar en el sistema de archivos donde un dispositivo de almacenamiento se une (o monta) al sistema de archivos. Es el punto donde los archivos y directorios almacenados en ese dispositivo de almacenamiento son accesibles para el sistema operativo y las aplicaciones.

Por ejemplo, en Windows las unidades de almacenamiento (como `

:`, `D:`, etc.) actúan como puntos de montaje principales para discos duros, unidades flash, unidades ópticas y otros dispositivos de almacenamiento.

Cuando creas un volumen nombrado, Docker asigna un punto de montaje específico en el sistema de archivos del host para ese volumen.

### Estructura del Punto de Montaje:
- /var/lib/docker/volumes/: Es la ubicación base donde Docker almacena todos los volúmenes en el sistema de archivos del host.
- nombreVolumen/: Es el nombre del volumen nombrado que has creado. Docker crea un directorio con este nombre dentro de /var/lib/docker/volumes/ para almacenar los datos del volumen.
- _data: Es el subdirectorio dentro de vol-postgres/ donde se almacenan los datos reales del volumen. El nombre _data es una convención utilizada por Docker para indicar el directorio donde se encuentran los datos del volumen.

### ¿Cómo acceder a ese Mountpoint?
En el contexto de WSL (Windows Subsystem for Linux), wsl$ se refiere al nombre de un recurso compartido de red especial que representa la raíz del sistema de archivos de Windows desde WSL. Cuando accedes a \\wsl$ desde el Explorador de archivos de Windows, puedes ver y acceder a los archivos del sistema de archivos de la distribución de Linux en WSL.
\\wsl.localhost\docker-desktop-data\data\docker\volumes

### Crear un contenedor vinculado a un volumen nombrado
```
docker run -d --name <nombre contenedor> -v <nombre volumen>:<ruta contenedor> <nombre imagen>
```
ó
```
docker run -d --name <nombre contenedor> --mount type=volume,src=<nombre >,dst=<mount-path>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje. Para volúmenes con nombre, este es el nombre del volumen. Para volúmenes anónimos, este campo se omite.


### Crear la red net-drupal de tipo bridge
# COMPLETAR CON EL COMANDO
docker network create net-drupal
<img width="1276" height="558" alt="image" src="https://github.com/user-attachments/assets/55a8dc61-a275-4226-8d02-02f79e1dae1f" />

### Crear un servidor postgres vinculado a la red net-drupal, completar la ruta del contenedor
```
docker run -d --name server-postgres -e POSTGRES_DB=db_drupal -e POSTGRES_PASSWORD=12345 -e POSTGRES_USER=user_drupal --network net-drupal postgres
```
_No es necesario exponer el puerto, debido a que nos vamos a conectar desde la misma red de docker_

### Crear un cliente postgres vinculado a la red drupal a partir de la imagen dpage/pgadmin4, completar el correo
```
docker run -d --name client-postgres --publish published=9500,target=80 -e PGADMIN_DEFAULT_PASSWORD=54321 -e PGADMIN_DEFAULT_EMAIL=<correo> --network net-drupal dpage/pgadmin4
```
<img width="2314" height="347" alt="image" src="https://github.com/user-attachments/assets/1950172f-3acc-4d06-8026-9b6b9d882e8d" />

### Usar el cliente postgres para conectarse al servidor postgres, para la conexión usar el nombre del servidor en lugar de la dirección IP.

<img width="2880" height="1524" alt="image" src="https://github.com/user-attachments/assets/ff691441-ec8e-4e7a-a45d-df47d1444ea6" />

### Crear los volúmenes necesarios para drupal, esto se puede encontrar en la documentación

### COMPLETAR CON LOS COMANDOS
	-v /path/on/host/modules:/var/www/html/modules \
	-v /path/on/host/profiles:/var/www/html/profiles \
	-v /path/on/host/sites:/var/www/html/sites \
	-v /path/on/host/themes:/var/www/html/themes \

### Crear el contenedor server-drupal vinculado a la red, usar la imagen drupal, y vincularlo a los volúmenes nombrados
```
docker run -d --name server-drupal --publish published=9700,target=80 -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> --network net-drupal drupal
```
<img width="2350" height="1225" alt="image" src="https://github.com/user-attachments/assets/d827c7fe-6f61-48d8-bded-21fe0ec167b5" />

### Ingrese al server-drupal y siga el paso a paso para la instalación.
# COMPLETAR CON UNA CAPTURA DE PANTALLA DEL PASO 4
<img width="2880" height="1524" alt="image" src="https://github.com/user-attachments/assets/c94162da-0e6f-4797-8880-4188c5d559fe" />

_La instalación puede tomar varios minutos, mientras espera realice un diagrama de los contenedores que ha creado en este apartado._

# COMPLETAR CON EL DIAGRAMA SOLICITADO
<img width="1038" height="917" alt="image" src="https://github.com/user-attachments/assets/3b2270da-32cb-48ba-a070-dc086ead537a" />

### Eliminar un volumen específico
```
docker volume rm <nombre volumen>
```
**Considerar**
Datos Persistentes: Asegúrate de que el volumen no contiene datos críticos antes de eliminarlo, ya que esta operación no se puede deshacer.
Contenedores Activos: No puedes eliminar un volumen que está actualmente en uso por un contenedor activo. Debes detener y/o eliminar el contenedor primero.
