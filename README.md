# Docker
Docker - PHP (5.6) - Phalcon (3.4.2) - Phalcon-devtools (3.4.8) - Memcached (2.2.0)

- Para la primera vez que se levanta el proyecto con docker o se cambie los archivos de docker ejecutar:
 
```bash
sudo docker-compose up --build -d
```

- En las siguientes oportunidades ejecutar:

Para levantar:
```bash
sudo docker-compose start
```
Para detener:
```bash
sudo docker-compose stop
```
- Para ingresar al contenedor ejecutar:
```bash
sudo docker-compose exec webserver bash
```
- Para verificar phalcon-devtools, dentro del contenedor ejecutar:
```bash
phalcon --help
```
- Si se tiene dependencias con composer, para instalarlas, dentro del contenedor con docker ejecutar:
```bash
composer install
``` 
- Para ver la web:

Sin virtualhost:
```bash
http://localhost:9087
```
Con virtualhost: Si se usa Linux, agregar en /etc/hosts de la pc host la siguiente linea:
```bash
177.28.114.10    local.phalcon.com
```