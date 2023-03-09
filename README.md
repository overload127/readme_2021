<h1 align="center">⚙️ RTS server and web client ⚙️</h1>

### Contents:

- [Server python](#server-python)
  - [Install backend as debug 🔨](#install-backend-as-debug-)
  - [Run backend as debug ▶️](#run-backend-as-debug-)
  - [Deploy backend on production server with caddy 🔨](#deploy-backend-on-production-server-with-caddy-)
- [Client nodejs react web](#client-nodejs-react-web)
  - [Install yarn 🔨](#install-yarn-)
  - [Install web client as debug 🔨](#install-web-client-as-debug-)
  - [Run web client as debug ▶️](#run-web-client-as-debug-)
  - [Deploy backend on production server with caddy 🔨](#Deploy-web-client-on-production-server-with-caddy-)

## Server python

### Install backend as debug 🔨

- Сhange folder to server
```bash
cd server
```
- Create python environment
```bash
python3.9 -m venv env
```
- Enter inenvironment
```bash
source ./env/bin/activate
```
- Install poetry with pip
```bash
pip install poetry
```
- Install dependency with poetry
```bash
poetry install
```

### Run backend as debug ▶️

- Run debug backend server
```bash
python -m uvicorn run_debug:app --reload --host 0.0.0.0 --port 8002
```

### Deploy backend on production server with caddy 🔨

- git clone git@gitlab.com:(НАШ_ПРИВАТНЫЙ_РЕПОЗИТОРИЙ).git
- Ставим python и caddy
- Копируем файлйл gunicorn.service и caddy.service из папки deploy в папку etc/systemd/system
- Указываем пользователя из под которого будем стартовать сервисы
- Коректируем пути в файлах *.service, т.к. они могут оличаться на разных машинах (обязательно убедится в наличии прав доступа у пользователя из под которого запускаются сервисы, на изменеие чтение и запуск файлов backend server-a)
- Тут же указываем место расположения логов. (обязательно убедится в наличии прав доступа у пользователя из под которого запускаются сервисы, на папку в которую будем писать логи)
- Так же копируем Caddyfile в папку /etc/caddy/ (получится /etc/caddy/Caddyfile)
- файл server/gunicorn.py соддержит так же пути до логов. Корректируем их под себя.
- Создаем папки для логов
- Добавляем в автозагрузку оба сервиса (sudo systemctl enable gunicorn) (sudo systemctl enable caddy)
- стартуем оба сервиса в первый раз вручную (sudo systemctl start gunicorn) (sudo systemctl start caddy)
- Проверяем статус сервисов. В случае проблем дебажим и исправляем ошибки (journalctl -u caddy.service --since today или journalctl -u gunicorn.service --since today)
