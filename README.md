## Guia rapida para crear nuevo proyecto

Dentro de la carpeta del nuevo proyecto, optando por usar como BD MySQL

1. ``` git clone git clone https://github.com/laradock/laradock.git ```
2. ``` cd laradock ```
3. ``` cp env-example .env ```
4. ``` docker-compose up -d nginx mysql ```
5. ``` docker-compose exec workspace bash ```
- Dentro del workspace container ejecutar: 
6. ``` composer create-project --prefer-dist laravel/laravel example ```
7. ``` exit ```
- Volvimos a nuestro directorio local:
8. Editar en el .env de laradock: ``` APP_CODE_PATH_HOST=../example ``` acorde al nombre de la carpeta de nuestro proyecto 
9. Editar el .env de nuestro ``` example ``` especificando DB Credentials (segun lo definido en .env de laradock). Por defecto acceso root es db=default, user=root, pass=root. Y (mucho muy importante)``` DB_HOST=mysql ```
Dentro de la carpeta laradock:
- Aca hay que hacer unas cosas locas por culpa de los permisos de acceso de mysql:
10. ``` nano laradock/mysql/my.cnf ``` (agregar a lo ultimo esto: ``` default_authentication_plugin=mysql_native_password ```)
11. ``` docker-compose exec mysql bash ```
12. (contaseña mysql root es root)
```
root@c2772502897c:/# mysql -u root -p

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
mysql> ALTER USER 'default'@'%' IDENTIFIED WITH mysql_native_password BY 'secret';
mysql> create database example;
mysql> exit

root@c2772502897c:/# exit
```
- Reseteamos los contenedores para que tome los cambios:
13. ``` docker-compose down ```
14. ``` docker-compose up -d nginx mysql ```
- Realizamos las migrañas:
15. ``` docker-compose exec workspace bash ```
16. ``` artisan migrate ```
17. Profit.