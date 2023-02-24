#INSTALACIÓN LARAVEL


1.Para instalar nuestro proyecto de Laravel tendremos que  situarnos en nuestro escritorio e introducir el siguiente comando.

`curl -s "https://laravel.build/mi-proyecto" | bash`

2.Después entramos dentro de esa carpeta.

`cd mi-proyecto`

3.para ARRANCAR SAIL haremos

`./vendor/bin/sail up -d`

!!!warning "CUIDADOO!"
    Tenemos que asegurarnos que otros contenedores de dockers estén parados.


https://desarrolloweb.com/articulos/laravel-sail

https://www.laraveltip.com/como-utilizar-laravel-con-docker-la-guia-definitiva/



## ¿Cómo podemos añadir nuestro PHPMYADMIN o ADMINER?

Tenemos que añadir en nuestro fichero ***docker-compose.yml*** y más abajo, en el último servicio añadir


Para poder acceder debemos poner **localhost:8080**

## ¿Cúal son nuestras contraseñas y usuarios de acceso?

En el fichero **.env** de nuestro proyecto tendremos todas las variables de sesión, es importante que no subamos este fichero en el github por seguridad.


https://ecwebservices.medium.com/adding-phpmyadmin-to-laravel-sail-64823687e084

https://ecwebservices.medium.com/adding-adminer-to-laravel-sail-727c80835106

___________________________