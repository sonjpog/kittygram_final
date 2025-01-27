![Main Kittygram workflow ](https://github.com/sonjpog/kittygram_final/actions/workflows/main.yml/badge.svg)


#  Учебный проект «Киттиграм»

Kittygram — социальная сеть для владельцев и любителей кошек. Здесь можно делиться фото питомцев и находить друзей с общими интересами.
Что умеет проект:

— Добавлять, просматривать, редактировать и удалять котиков.
— Добавлять новые и присваивать уже существующие достижения.
— Просматривать чужих котов и их достижения.


## Установка проекта

1. Клонируйте репозиторий на свой компьютер
```yaml
https://github.com/sonjpog/kittygram_final.git
```

2. Создайте файл .env в корневой директории проекта. 

3. Внесите в него значения переменных окружения: 
```yaml
POSTGRES_DB=<БазаДанных>
POSTGRES_USER=<имя пользователя>
POSTGRES_PASSWORD=<пароль>
DB_NAME=<имя БазыДанных>
DB_HOST=db
DB_PORT=5432
SECRET_KEY=<ключ Django>
DEBUG=<DEBUG True/False>
ALLOWED_HOSTS=<разрешенные хосты>
```

## Создание Docker-образов

1. Замените username на ваш логин на DockerHub:
```yaml
cd frontend
docker build -t username/kittygram_frontend .
cd ../backend
docker build -t username/kittygram_backend .
cd ../nginx
docker build -t username/kittygram_gateway . 
```

2. Загрузите образы на DockerHub:
```yaml
docker push username/kittygram_frontend
docker push username/kittygram_backend
docker push username/kittygram_gateway
```

## Запуск проекта на сервере

1. Подключитесь к удаленному серверу: 
```yaml
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
```

2. Создайте на сервере директорию kittygram: 
```yaml
mkdir kittygram
```

3. Установите docker compose на сервер:
```yaml
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```

4. В директорию kittygram/ скопируйте файлы docker-compose.production.yml и .env любым удобным способом. 

5. Запустите docker compose в режиме демона:
```yaml
sudo docker compose -f docker-compose.production.yml up -d
```

6. Выполните миграции, соберите статические файлы бэкенда и скопируйте их в /backend_static/static/:
```yaml
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

7. На сервере в редакторе nano откройте конфиг Nginx:
```yaml
sudo nano /etc/nginx/sites-enabled/default
```

8. Измените настройки location в секции server:
```yaml
location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}
```

9. Перезапустите Nginx:
```yaml
sudo service nginx reload
```

## Настройте запуск проекта Kittygram в контейнерах и CI/CD с помощью GitHub Actions

Заполните переменные в разделе Actions secrets
```yaml
DOCKER_PASSWORD - пароль от Docker Hub
DOCKER_REPOSITORY - имя пользователя в GitHub
DOCKER_USERNAME - имя пользователя Docker Hub
HOST - ip сервера
SSH_KEY - ключ ssh для доступа к удаленному серверу
SSH_PASSPHRASE - пароль ssh
TELEGRAM_TO - id пользователя TELEGRAM
TELEGRAM_TOKEN - TELEGRAM токен
USER - имя пользователя сервера

```

## Стек технологий

![django](https://camo.githubusercontent.com/5cc076a62f7189d22260996c9bec5ca6eef5e5b537f62e64b68e3028d0de29b5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446a616e676f2d3039324532303f6c6f676f3d646a616e676f266c6f676f436f6c6f723d7768697465) ![postgresql](https://camo.githubusercontent.com/0c918464c578d8cd8f9be93baaabc8e84b94d8f03efe4e3f18799e2c3de395f8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f506f737467726553514c2d3333363739313f6c6f676f3d706f737467726573716c266c6f676f436f6c6f723d7768697465) ![Nginx](https://camo.githubusercontent.com/cd6eba769dc7617401e02baea47325fdd47b841c920acccca94f411020f9d507/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4e67696e782d3030393633393f6c6f676f3d6e67696e78266c6f676f436f6c6f723d7768697465) ![Docker](https://camo.githubusercontent.com/0bba72db052794db7b917abda181702a7af7d352e704868f8b0eef45304d7dcf/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f636b65722d3234393645443f6c6f676f3d646f636b6572266c6f676f436f6c6f723d7768697465) ![GitHubActions](https://camo.githubusercontent.com/46ced4d91ace1d06b77689c0ba6d10688b02219fef3f21dfe8b0a95067ad16cf/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4769744875625f416374696f6e732d3230383846463f6c6f676f3d6769746875622d616374696f6e73266c6f676f436f6c6f723d7768697465)

Работу выполнила [Софья Погосян](https://github.com/sonjpog)