## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp
# COMPLETAR CON EL COMANDO 
docker network create net-wp

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql

Ruta carpeta host: .../ejercicio3/db

### ¿Qué contiene la carpeta db del host?
Esta vacía
# COMPLETAR CON LA RESPUESTA A LA PREGUNTA

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
# COMPLETAR CON EL COMANDO
docker run -d --name srv-mysql -v C:\Users\emiau\Downloads\nginx\ejercicio3\db:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=emiau123 -e MYSQL_USER=emiau -e MYSQL_PASSWORD=emiau123 -e MYSQL_DATABASE=db_wordpress mysql:8<img width="2876" height="869" alt="image" src="https://github.com/user-attachments/assets/b7209d87-754c-4a62-ace6-04a9ab6e4ffe" />

### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
# COMPLETAR CON LA RESPUESTA A LA PREGUNTA
<img width="2880" height="1524" alt="image" src="https://github.com/user-attachments/assets/498663ac-0ae5-4f33-b25d-2a01623228cd" />

### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es /var/www/html

Ruta carpeta host: .../ejercicio3/www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)
# COMPLETAR CON EL COMANDO
docker run -d --name srv-wordpress -p 9500:80 --link srv-mysql:mysql_db -e WORDPRESS_DB_HOST=mysql_db -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=emiau123 -e WORDPRESS_DB_NAME=db_wordpress -v C:\Users\emiau\Downloads\ejercicio3\www:/var/www/html wordpress
<img width="2879" height="981" alt="image" src="https://github.com/user-attachments/assets/85ea2613-6dba-424e-88a8-46b7dcc6ecd6" />

### Personalizar la apariencia de wordpress y agregar una entrada
<img width="2880" height="1524" alt="image" src="https://github.com/user-attachments/assets/9f1fe10f-8c3c-42d9-8863-abeba2cc3c6b" />

### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?
Sigue la pagian configurada he intacta.
# COMPLETAR CON LA RESPUESTA A LA PREGUNTA 
<img width="2880" height="1524" alt="image" src="https://github.com/user-attachments/assets/73a464b3-9dcb-4be6-b8ac-73ede7ddb1cc" />

