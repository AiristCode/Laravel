#### #### **Guide**

1. Create Folder Laravel
```bash
mkdir laravel
```

2. Clone Repository Perpus
```bash
git clone https://github.com/academynusa/perpus-laravel.git
```

3. Create Docker file
```bash
vi > :set paste > i (Insert mode paste)
    FROM php:7.4-fpm-alpine

    RUN docker-php-ext-install pdo pdo_mysql sockets
    RUN curl -sS https://getcomposer.org/installer​ | php -- \
        --install-dir=/usr/local/bin --filename=composer

    COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

    WORKDIR /app
    COPY . .
```

4. Create Docker Image
```bash
docker build -t perpus-laravel .
--
docker images | grep perpus
```

5. Create Network
```bash
docker network create --driver=bridge laravel-net
```

6. Run Container Laravel and Mysql
```bash
docker run -d -p 8000:8000 --name perpus-app --net laravel-net perpus-laravel
docker run -d -p 3306:3306 --name my-mysql --net laravel-net -e MYSQL_DATABASE=homestead -e MYSQL_USER=homestead -e MYSQL_PASSWORD=secret -e MYSQL_ROOT_PASSWORD=secret  mysql:5.7
```

7. Run in Container laravel
```bash
# Check container
docker ps
# masuk container id laravel(perpus-laravel)
docker exec -it <CONTAINER_ID> /bin/sh
```

8. Run this in container
```bash
# folder di /app
cd perpus-laravel/
# install and update package
composer update
# copy environtment
cp .env.example .env
# change the environment
vi .env
DB_CONNECTION=mysql
DB_HOST=my-mysql
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
# php artisan generate key, migrate, seeder database
php artisan key:generate
php artisan migrate
php artisan db:seed
php artisan serve --host=0.0.0.0
```


