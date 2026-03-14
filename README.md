# Docker Environments For Common Development Setups

Bo thu muc nay gom nhieu moi truong Docker tach rieng. Moi thu muc chi chua 1 `docker-compose.yml` va moi file compose chi khoi tao 1 container chinh cho dung muc dich.

Phu hop voi may Windows 10, AMD Ryzen 5 3400G, RAM 16GB. Cau hinh nay du de chay cac moi truong phat trien pho thong, nhung khong nen mo tat ca container cung luc neu khong can.

## Cau truc thu muc

```text
docker-envs/
  mysql/
  postgres/
  nodejs/
  php-laravel/
  python-flask/
  golang/
  java-spring/
  redis/
  nginx/
  mongodb/
  phpmyadmin/
  adminer/
```

## Yeu cau

- Cai [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Tren Windows 10, nen bat WSL2 backend trong Docker Desktop de hieu nang tot hon
- Mo terminal tai dung thu muc moi truong truoc khi chay lenh

## Cach dung chung

Di chuyen vao thu muc moi truong:

```bash
cd docker-envs/mysql
docker compose up -d
```

Tat moi truong:

```bash
docker compose down
```

Xem log:

```bash
docker compose logs -f
```

## Cach mount source code tren Windows

Co 2 cach pho bien:

### Cach 1: Mount bang volume trong `docker-compose.yml`

Vi du:

```yaml
volumes:
  - D:/projects/my-laravel-app:/var/www/html
```

Hoac:

```yaml
volumes:
  - ./app:/app
```

Neu source code nam cung cap voi file compose, nen dung dang `./ten-thu-muc` de de mang sang may khac.

### Cach 2: Mount tam bang lenh `docker run`

Vi du:

```bash
docker run --rm -it -v D:/projects/my-node-app:/app -w /app node:latest bash
```

Trong bo thu muc nay, minh uu tien dung `docker compose` de ban quan ly don gian hon.

## Chon container theo loai du an

### 1. Laravel

Nen dung: `docker-envs/php-laravel`

Muc dich:
- Chay PHP CLI, Composer, Artisan
- Mount source Laravel vao `/var/www/html`

Vi du mount:

```yaml
volumes:
  - D:/projects/my-laravel-app:/var/www/html
```

Lenh thuong dung:

```bash
docker compose run --rm php composer install
docker compose run --rm php php artisan key:generate
docker compose run --rm php php artisan migrate
docker compose run --rm php php artisan serve --host=0.0.0.0 --port=8000
```

Luu y:
- Laravel thuong can them DB, Redis, queue. Ban co the chay them `mysql`, `postgres`, `redis` neu can.
- File compose nay tap trung vao PHP container. Neu can web server rieng, ban co the dung them `nginx`.

### 2. PHP thuan / WordPress / Symfony

Nen dung: `docker-envs/php-laravel`

Vi du mount:

```yaml
volumes:
  - D:/projects/my-php-app:/var/www/html
```

### 3. Node.js / Express / NestJS / Next.js

Nen dung: `docker-envs/nodejs`

Vi du mount:

```yaml
volumes:
  - D:/projects/my-node-app:/app
```

Lenh thuong dung:

```bash
docker compose run --rm node npm install
docker compose run --rm --service-ports node npm run dev
```

### 4. Python / Flask / FastAPI / Django

Nen dung: `docker-envs/python-flask`

Vi du mount:

```yaml
volumes:
  - D:/projects/my-python-app:/app
```

Lenh thuong dung:

```bash
docker compose run --rm python pip install -r requirements.txt
docker compose run --rm --service-ports python flask run --host=0.0.0.0 --port=5000
```

Neu dung FastAPI:

```bash
docker compose run --rm --service-ports python uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

### 5. Database MySQL

Nen dung: `docker-envs/mysql`

Thong tin mac dinh:
- User: `root`
- Password: `12345678`
- Port: `3306`

### 6. Database PostgreSQL

Nen dung: `docker-envs/postgres`

Thong tin mac dinh:
- User: `postgres`
- Password: `12345678`
- Port: `5432`

### 7. Redis

Nen dung: `docker-envs/redis`

Thuong di kem:
- Laravel queue/cache
- Node queue/cache
- Python Celery/cache

### 8. MongoDB

Nen dung: `docker-envs/mongodb`

Thuong di kem:
- Node.js
- Python
- Ung dung NoSQL

### 9. phpMyAdmin

Nen dung: `docker-envs/phpmyadmin`

Dung de quan ly MySQL bang giao dien web.

### 10. Adminer

Nen dung: `docker-envs/adminer`

Dung de quan ly MySQL/PostgreSQL bang giao dien web gon nhe.

### 11. Nginx

Nen dung: `docker-envs/nginx`

Dung khi:
- Ban muon reverse proxy
- Ban muon serve PHP/Laravel thong qua web server rieng
- Ban muon map source static site

### 12. Golang

Nen dung: `docker-envs/golang`

Thuong di kem:
- API Go
- CLI tools
- Microservices

### 13. Java / Spring Boot

Nen dung: `docker-envs/java-spring`

Thuong di kem:
- Spring Boot API
- Java backend thong dung
- Maven co san trong container

## Goi y phan bo tai nguyen cho may cua ban

Voi Windows 10, Ryzen 5 3400G, RAM 16GB:

- Docker Desktop RAM: nen dat 6GB den 8GB
- CPU: 4 core la hop ly
- Khong nen chay dong thoi qua nhieu DB containers neu khong can
- Khi dev, uu tien chi bat nhung container dang dung

## Cach chay tung moi truong

### MySQL

```bash
cd docker-envs/mysql
docker compose up -d
```

Ket noi:
- Host: `127.0.0.1`
- Port: `3306`
- User: `root`
- Password: `12345678`

### PostgreSQL

```bash
cd docker-envs/postgres
docker compose up -d
```

Ket noi:
- Host: `127.0.0.1`
- Port: `5432`
- User: `postgres`
- Password: `12345678`

### Node.js

```bash
cd docker-envs/nodejs
docker compose run --rm node bash
```

Neu muon mount source, sua volume trong file compose truoc.

### PHP / Laravel

```bash
cd docker-envs/php-laravel
docker compose run --rm php bash
```

### Python / Flask

```bash
cd docker-envs/python-flask
docker compose run --rm python bash
```

### Golang

```bash
cd docker-envs/golang
docker compose run --rm golang bash
```

Port mac dinh goi y:
- Host: `8082`
- Container: `8080`

### Java / Spring Boot

```bash
cd docker-envs/java-spring
docker compose run --rm java bash
```

Port mac dinh goi y:
- Host: `8083`
- Container: `8080`

### Redis

```bash
cd docker-envs/redis
docker compose up -d
```

### MongoDB

```bash
cd docker-envs/mongodb
docker compose up -d
```

### phpMyAdmin

```bash
cd docker-envs/phpmyadmin
docker compose up -d
```

Truy cap:
- [http://localhost:8080](http://localhost:8080)

### Adminer

```bash
cd docker-envs/adminer
docker compose up -d
```

Truy cap:
- [http://localhost:8081](http://localhost:8081)

### Nginx

```bash
cd docker-envs/nginx
docker compose up -d
```

Truy cap:
- [http://localhost:8088](http://localhost:8088)

## Goi y ket hop nhanh

### Laravel co MySQL

Chay:
- `docker-envs/php-laravel`
- `docker-envs/mysql`

Mount source Laravel vao:
- `/var/www/html`

DB host trong Laravel `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=host.docker.internal
DB_PORT=3306
DB_DATABASE=app
DB_USERNAME=root
DB_PASSWORD=12345678
```

### Laravel co PostgreSQL

Chay:
- `docker-envs/php-laravel`
- `docker-envs/postgres`

DB host:

```env
DB_HOST=host.docker.internal
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=12345678
```

### Node.js co MongoDB

Chay:
- `docker-envs/nodejs`
- `docker-envs/mongodb`

Connection string:

```text
mongodb://root:12345678@host.docker.internal:27017
```

### Flask co PostgreSQL

Chay:
- `docker-envs/python-flask`
- `docker-envs/postgres`

### Go API

Chay:
- `docker-envs/golang`

Mount source vao:
- `/app`

### Spring Boot API

Chay:
- `docker-envs/java-spring`

Mount source vao:
- `/workspace`

## Luu y thuc te

- `latest` la tag bien dong. Hom nay ban dung duoc, nhung mai co the image thay doi.
- Rieng `php-laravel`, minh dung PHP 8.3 thay vi `latest` de hop voi Laravel pho thong va giam rui ro vo tuong thich.
- Neu muon on dinh lau dai, sau nay ban nen khoa version cu the.
- Mot so moi truong da doi host port de tranh trung cong khi mo nhieu container cung luc.
- Tren Windows, neu code nam o o dia qua cham, hieu nang mount co the giam. Thu muc trong WSL2 thuong cho toc do tot hon.
