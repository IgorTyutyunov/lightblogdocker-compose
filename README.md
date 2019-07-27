# lightblogdocker-compose
Ссылка на образ ROR проекта: https://cloud.docker.com/repository/docker/igortyutyunov/lightblog

Ссылка на проект на github: https://github.com/IgorTyutyunov/LightBlogDocker


Инструкция:
1) создаём дирректорию для Dockerfile, docker-compose.yml и .evn: mkdir имя_каталога.

2) создаём в дирректории файл Dockerfile со следующим содержимым:
FROM igortyutyunov/lightblog:v3

3) создаём в дирректории файл docker-compose.yml со следующим содержимым:
version: "3"

services:
  db:
    image: mysql:5.7
    restart: always
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
      - .:/myapp
    environment:
      MYSQL_ROOT_PASSWORD: ${DATABASE_PASSWORD}
      MYSQL_USER: ${DATABASE_USER}
      MYSQL_PASSWORD: ${DATABASE_PASSWORD}
  web:
    image: igortyutyunov/lightblog:v3
    build: .
    command: bundle exec rails s -p 3000 -b "0.0.0.0"
    env_file:
      - .env
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
    stdin_open: true
    tty: true
volumes:
  db_data:

4) создаём в дирректории файл .env со следующим содержимым: 
DATABASE_USER=root
DATABASE_PASSWORD=password
DATABASE_HOST=db

5) в каталоге c файлами выполняем команду: docker-compose up –build
6) открываем новое окно терминала, переходим в созданный  каталог с файлами, и в нём выполняем команды:
docker-compose run web rake db:create
docker-compose run web rake db:migrate

6) Открываем браузер и переходим по адресу: http://localhost:3000/
