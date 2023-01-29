Adaptado de.
https://dev.to/veevidify/docker-compose-up-your-entire-laravel-apache-mysql-development-environment-45ea

1. Clonar repo laravel vacío

	1. Crear repo en github
	2. Clonar repo vacio en local
	3. Clonar repo en otra carpeta  `git clone https://github.com/laravel/laravel.git`
	4. Copiar el contenido de 3 en 2 ex
	5. git add --all
	6. git commit -m "first commit" 
	7. git push
   
2. Reescribir carpeta de propiedades del entorno

 `cp .env.example .env`

1. Crear carpeta de nombre run
2. Añadir al .gitignore la carpeta run/var
3. Crear .dockerignore y añadir la carpeta run/var

4. Crear dockerfile y copiar
5. Crear docker-compose.yaml y copiar

6. Crear nuevo repositorio si procede
7. Camabiar remote del repositorio y hacer primer commit
	Cambiar el remote de github
	https://careerkarma.com/blog/git-change-remote/

8.  Hacer mysql dump si procede, 
	10.1 ir al binario de mysql: cd C:\Program Files\MySQL\MySQL Server 5.5\bin
	10.2 lanzar el binario de mysqldump, hay distintos formatos pero quedaros con el pipe al fichero de salida , 
		el user y la base de datos concreta que queramos copiar.
		mysqldump –e –u[username] -p[password] -h[hostname] [database name] > C:\[filename].sql
		mysqldump -u user -p database > backup.sql
		
		si el dump falla a lo mejor falta la creación de una bd 
			CREATE DATABASE IF NOT EXISTS dbname;

9.  Si es una aplicación vacía
		Generar datos y fichero mysql dump
		https://filldb.info/	

10. Configurar el user id UID=1000 en .env 
	https://www.compuhoy.com/quien-es-el-usuario-1000-de-linux/

11. Conectar el puerto 3306 de contenedor mysql a un puerto local para manipular base de datos desde nuestro equipo
12. 
13. Añadir contenedor de Php My admin si procede

14. Hacer el build del docker compose
	`docker-compose build`
	
15. Si el build ha ido bien lo levantamos 
	`docker-compose up -d`
	1.  para hacer un nuevo build si cambios en la config tira el actuar
	`docker-compose down`
	2.  Para ver contenedores activos
	`docker ps` 
		
	
17.	Abrir una terminal en el contendor de php
	`docker exec -it laravel-app bash -c "sudo -u` `devuser /bin/bash"`
	`docker exec -u 0 -it mycontainer bash `
	`composer install`


18. Si nos falla la version de php minima podemos probar lo siguiente
	18.a Buscarn una imagen base nueva, reeplazar primera intrucción por 
		`php:8.2.1-apache`


	18.b Borrar containers
		
		`docker system prune -a`
		`docker-compose up -d --force-recreate --build`
		`docker-compose pull`
		
	18.c Si falla al añadir extensiones probar con:
		`RUN apt-get update`
		`RUN apt-get install libonig-dev`
		`RUN apt-get install -y libzip-dev `

19. Generar claves de artisan
	https://stillat.com/blog/2016/12/07/laravel-artisan-key-command-the-keygenerate-command
	`php artisan key:generate`
	
20.  `php artisan migrate`
	
	1.  Si esto falla es que tenemos el puerto de antes bloqueado o nos hemos equivocado en configurar 
	el fichero .env

21. php artisan make:auth
	`composer require laravel/ui --dev`
	
22.`php artisan ui vue --auth`
	
	Parece que en este punto todavía al no tener que estar levantado el mysql no se puede migrar pero 
	se deberia poder hacer composer install y artisan key generate sin problema.
	
		`RUN npm install`
		`WORKDIR /var/www/html`
		`COPY . /var/www/html/`
		`RUN npm install`		
		`ENV COMPOSER_ALLOW_SUPERUSER=1`
		`RUN composer install`
		`RUN composer require laravel/ui --dev`
		`RUN php artisan key:generate`
		`RUN php artisan ui vue --auth`
		`RUN npm install && npm run dev`
		 -- `RUN php artisan migrate` Esta lanzar a mano mejor
		`RUN php artisan make:auth`
		`RUN npm install && npm run dev`

23. So os falla la ui al hacer login, puede ser que tengais una version de node antigua
	para instalar una version de node nueva, como super usuario hacer, probad en la consola del contenedor,
	si funciona integradlo al Dockerfile
	
	`npm install -g n`
	`n stable`

24. Para levantar un contenedro de mysql básico
25. 
`docker run --name basic-mysql --rm -v /tmp/mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=ANSKk08aPEDbFjDO -e MYSQL_DATABASE=testing -p 3306:3306 -it mysql:8.0`


24. Si teneis problemas de autenticación poned con comillas password y usuario en .env

25. Para lanzar el servidor poner 
	`npm run dev` o
	`npm run dev -- --host`

26. Para tagear un contenedor


27. Para bajarse una imagen de dockerhub - repo:tag

`docker pull mikeldalmauc/portfolio:first`


27. Para conectar un servidor a una red de docker
`docker network connect proxy myadmin`

28. Para asociar un contenedor a un dominio, esas variables estaán definidas por eso domain debería sustituirse

`docker run -d -e VIRTUAL_HOST=$DOMAIN --network=$NETWORK --rm --name micontenedor httpd:alpine`

29. Para levantar un contenedor con link otro
    
`docker run --name myadmin -d --link basic-mysql:db -p 8080:80 phpmyadmin`


https://stackoverflow.com/questions/34545641/php-artisan-makeauth-command-is-not-defined
https://www.docker.com/blog/how-to-deploy-on-remote-docker-hosts-with-docker-compose/
https://docs.docker.com/engine/context/working-with-contexts/
https://docs.docker.com/compose/production/
https://dev.to/veevidify/docker-compose-up-your-entire-laravel-apache-mysql-development-environment-45ea
https://francoisromain.medium.com/host-multiple-websites-with-https-inside-docker-containers-on-a-single-server-18467484ab95
https://github.com/evertramos/nginx-proxy-automation
Utils Installation Srcipts: https://www.youtube.com/watch?v=eiTivLeIkm0
Server Automation: https://github.com/evertramos/server-automation
https://medium.com/codex/load-balancing-with-nginx-7d1bb4c24d5d
Automated Deployment: https://levelup.gitconnected.com/automated-deployment-using-docker-github-actions-and-webhooks-54018fc12e32
Github Worklows: https://docs.github.com/en/actions/using-workflows#creating-a-workflow-file
https://phoenixnap.com/kb/mysql-docker-container
https://docs.docker.com/network/bridge/
