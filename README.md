# Пошаговая инструкция выполнения 3й лабораторной работы по Сетевым, от Романа

На выполнение всей этой работы, лично у меня ушло **25 часов** непрерывного времени *(за 5 дней)*

---

! По ходу выполнения задания, нужно делать скриншоты !

---

## Скачивание дистрибутива Ubuntu Linux 

Скачивание дистрибутива Ubuntu Linux с сайта https://www.osboxes.org/ubuntu/ 

Установка этой системы через VirtualBox 
Используя эту инструкцию:

- Разархивируете файл .vdi, он будет весить 5ГБ
- В VirtualBox нажимаете кнопку "Создать"
- Записываете имя новой машины, он сам подставит нужные параметры, нажимаете далее
- Из 3х опций - выбираете среднюю, и указываете путь к этому файлу .vdi
- Там будет поле выбора, и маленькая кнопка справа от этого поля. Нажимаете её, и выбираете там в проводнике этот файл .vdi
- Всё готово, запускаете систему

---

**Имя пользователя:** `osboxes`
**Пароль:** `osboxes.org`

---

## Настройка системы

#### Обновление системы:

```
sudo apt update
sudo apt upgrade
```

1. команда займёт 5 секунд
2. команда займёт 25 ***минут***

---

#### Создание своего пользователя:

sudo adduser 
И потом в настройках находите пользователя, и устанавливаете его как администратора, для удобства
С паролем придётся повозиться, ну что поделать

---

#### Изменение разрешения экрана:

В настройках находите Display, и выбираете новое разрешение
Затем нужно нажать красную кнопку Apply в правом верхнем углу

---

#### Включение буфера обмена с основной системой:

https://www.youtube.com/watch?v=0drVeZlP-Ck

---

#### Русификация:

sudo su

Для русификации интерфейса в Ubuntu вы можете следовать следующим шагам:

Установите пакеты русификации для установленных программ:
sudo apt-get install language-pack-ru
sudo apt-get install language-pack-gnome-ru
sudo apt-get install language-pack-kde-ru

Установите переводы для пользовательских программ:
sudo apt-get install firefox-locale-ru

* Команды, которые выполняются долго:

  - sudo apt-get install libreoffice-l10n-ru libreoffice-help-ru

  - sudo apt-get install thunderbird-locale-ru

  - sudo apt-get install gimp-help-ru

Установите русский словарь для проверки орфографии и тезаурус (словарь синонимов, антонимов и др.) для LibreOffice/OpenOffice.org:
sudo apt-get install hunspell-ru
sudo apt-get install mythes-ru

- Откройте файл /etc/default/locale с помощью текстового редактора и замените его содержимое на LANG=ru_RU.UTF-8:

  sudo gedit /etc/default/locale

  Сохраните файл и перезайдите в систему.

* Либо же можно переставить язык на Русский, в настройках

---

Теперь, после русификации системы, вы можете спокойно полазить по настройкам, и настроить нужные опции, для удобной работы (например, там есть прикольные фоны для рабочего стола, с минотаврами)

---

Вы можете сделать снимок виртуальной машины (считай, резервную копию), если хотите

---

---

# Теперь можно переходить к выполнению задания по Сетевым:

Я буду указывать команды, и комментарии к ним, по шагам. Пошаговую инструкцию скидывал нам Роман, но я продублирую её здесь (она прикреплена рядом)

Шаги прописаны в файле от Романа.

## 2 шаг

Вот шаги, которые вам нужно выполнить:

1. **Установите UFW и OpenSSH**. Это можно сделать с помощью следующих команд:

```bash
sudo apt-get update
sudo apt-get install ufw
sudo apt-get install openssh-server
```

2. **Разрешите доступ для OpenSSH в UFW**. Это можно сделать с помощью следующей команды:

```bash
sudo ufw allow OpenSSH
```

3. **Измените стандартный порт для подключения по SSH (22) на любой другой**. Для этого отредактируйте файл конфигурации SSHD:

```bash
sudo nano /etc/ssh/sshd_config
```

Затем найдите строку, которая начинается с `#Port 22`, удалите символ `#` и измените `22` на желаемый номер порта. Например, `Port 24`.

4. **Запретите подключение через root-пользователя**. В том же файле конфигурации найдите строку `PermitRootLogin` и установите её значение на `no`.

5. **Создайте нового пользователя и дайте ему права админа**. Это можно сделать с помощью следующих команд:

```bash
sudo adduser sshuser
sudo usermod -aG sudo sshuser
```

Замените `newuser` на имя пользователя, которое вы хотите использовать.

6. **Запустите UFW**. Это можно сделать с помощью следующей команды:

```bash
sudo ufw enable
```

---

## 3 шаг

Вот шаги, которые вам нужно выполнить:

1. **Установите PostgreSQL**. Это можно сделать с помощью следующих команд:

```bash
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
```

2. **Создайте пользователя базы данных**. Это можно сделать с помощью следующих команд:

После введения команды ниже, введите имя пользователя, когда вас об этом попросят, и ответьте “n” на все вопросы о разрешениях:

```bash
sudo -u postgres createuser --interactive
```

​	Я назвал пользователя `User2`

3. **Создайте пустую базу данных с названием “keycloak”**. Это можно сделать с помощью следующей команды:

```bash
sudo -u postgres createdb keycloak
```

4. **Измените стандартный порт подключения к PostgreSQL**. Для этого отредактируйте файл конфигурации PostgreSQL:

```bash
sudo nano /etc/postgresql/15/main/postgresql.conf
```

Затем найдите строку, которая начинается с `#port = 5432`, удалите символ `#` и измените `5432` на желаемый номер порта. Например, `port = 5430` - Я не уверен, что нужно поставить именно этот порт

5. **Запустите Postgres на localhost:**

```bash
systemctl start postgresql
```

Проверить, запустился ли Postgres можно этой командой:

```bash
sudo systemctl status postgresql
```

---



## 4 шаг

Вот шаги, которые вам нужно выполнить:

1. **Установите Docker**. Это можно сделать с помощью следующих команд:

```bash
sudo apt-get install docker-ce
```

2. **Проверьте, что Docker работает**. Это можно сделать с помощью следующей команды:

```bash
sudo systemctl status docker
```

3. **Скачайте Keycloak в виде архива с официального сайта**

Создаёте и переходите в нужную директорию. Например, я сделал так:

```bash
sudo su
mkdir keylock_tmp
cd ./keylock_tmp
```

Затем скачиваете, и распаковываете архив keycloak-а последней версии (я взял ссылку с оф сайта):

```bash
wget https://github.com/keycloak/keycloak/releases/download/23.0.1/keycloak-17.0.1.zip
```

``` bash
unzip keycloak-17.0.1.zip 
sudo apt-get install unzip
```

4. **Создайте Dockerfile в той же директории, куда распаковали Keycloak**. Это можно сделать с помощью следующей команды:

```bash
nano Dockerfile
```

Затем вставьте следующий код в Dockerfile:

```bash
FROM quay.io/keycloak/keycloak:latest as builder

# Enable health and metrics support
ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true

# Configure a database vendor
ENV KC_DB=postgres

WORKDIR /opt/keycloak

# for demonstration purposes only, please make sure to use proper certificates in production instead
RUN keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore

RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:latest

COPY --from=builder /opt/keycloak/ /opt/keycloak/

# change these values to point to a running postgres instance
ENV KC_DB=postgres
ENV KC_DB_URL=jdbc:postgresql://localhost:5430/keycloak
ENV KC_DB_USERNAME=User2
ENV KC_DB_PASSWORD=Pass24
ENV KC_HOSTNAME=localhost

ENTRYPOINT ["/opt/keycloak/bin/kc.sh"]
```

Убедитесь, что данные в инструкциях `ENV` верные, и соответствуют тем данным, которые вы вводили при создании Postgres.

5. **Соберите образ Docker**. Это можно сделать с помощью следующей команды:

```bash
sudo docker build -t keycloak_image .
```

Пересобирать образ докера нужно каждый раз, после изменений в его конфигурации (что логично)

---

**Запуск контейнера Docker**. Это можно сделать с помощью следующей команды:

**Команда для запуска в обычном режиме:**

```bash
docker run -p 8080:8080 --restart always --network=host -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=Pass24 quay.io/keycloak/keycloak:17.0.1
```

*Лично у меня эта команда не работала*

**Команда для запуска в режиме разработчика:**

```bash
docker run -p 8080:8080 --restart always --network=host -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=Pass24 quay.io/keycloak/keycloak:17.0.1 start-dev
```

Не до конца уверен, что эту часть нужно использовать. Но она вроде как открывает доступ из контейнера для keycloak к localhost в системе, тогда как без этой команды localhost будет только внутри контейнера. По идее, она нужна (в составе предыдущей команды):

```
--network=host
```

---

Далее, нужно будет запустить Keycloak, внутри Docker. Для этого, нужно выполнить команду запуска, внутри Docker-контейнера, а не в обычной консоли.

Для начала, нужно узнать ID запущенного контейнера

Посмотреть все запущенные контейнеры, в данный момент:

```bash
docker ps
```

Для выполнения команд внутри контейнера, по его ID, нужна команда:

```bash
docker exec -it ___________
```

Где ____ - это его ID. Например:

```bash
docker exec -it 532d690cb72d /opt/keycloak/bin/kc.sh start
```

---

При неправильном запуске контейнера, Docker не удаляет (не останавливает) его автоматически, и он забивает хост 8080, и не даст запустить вам контейнер ещё раз на этом хосте, после изменении конфигурации. По этому эта информация ↓

#### Может пригодиться:

Посмотреть все запущенные контейнеры, в данный момент:

```bash
docker ps
```

Остановить процесс контейнера, по его ID:

```bash
docker stop [Номер контейнера] 
```

Отключение автозапуска у контейнера:

```bash
docker update --restart=no [Номер контейнера] 
```

Экспорт контейнера (может пригодиться):

```bash
docker export container_name > container.tar
```

Удаление контейнера (что бы он не запускался, ну и в принципе очистить пространство контейнеров, если вы их там наплодили 10 штук и больше). Работает только после остановки контейнера

```bash
docker rm [Номер контейнера] 
```

---

8. **Заходите на localhost**, и любуйтесь своим результатом

```
http://localhost:8080/
```

Если вы настроили команду старта докер-контейнера правильно (а именно, не забыли там `--restart always`), то Keycloak на localhost будет доступен и после перезапуска виртуальной машины.

---

**Подсказка:** Если вы заходите на localhost, и видите, что там *нет* ваших realm-ов, и сохранённых данных, то посмотрите в консоли, командой `docker ps`, нет ли там нескольких запущенных контейнеров с Keycloak. Такое может произойти, если вы что-то там химичили (как, например, я). Если есть, остановите все, кроме самого первого (он был создан позже всех)

---

Вот такое сообщение, с информацией о **Keycloak** отобразится в консоли:

```bash
root@osboxes:/home/gogik/keylock_tmp# sudo docker run -p 8080:8080 --restart always keycloak_image
Keycloak - Open Source Identity and Access Management

Find more information at: https://www.keycloak.org/docs/latest

Usage:

kc.sh [OPTIONS] [COMMAND]

Use this command-line tool to manage your Keycloak cluster.

Options:

-cf, --config-file <file>
                     Set the path to a configuration file. By default, configuration properties are
                       read from the "keycloak.conf" file in the "conf" directory.
-h, --help           This help message.
-v, --verbose        Print out error details when running this command.
-V, --version        Show version information

Commands:

  build                   Creates a new and optimized server image.
  start                   Start the server.
  start-dev               Start the server in development mode.
  export                  Export data from realms to a file or directory.
  import                  Import data from a directory or a file.
  show-config             Print out the current configuration.
  tools                   Utilities for use and interaction with the server.
    completion            Generate bash/zsh completion script for kc.sh.

Examples:

  Start the server in development mode for local development or testing:

      $ kc.sh start-dev

  Building an optimized server runtime:

      $ kc.sh build <OPTIONS>

  Start the server in production mode:

      $ kc.sh start <OPTIONS>

  Enable auto-completion to bash/zsh:

      $ source <(kc.sh tools completion)

  Please, take a look at the documentation for more details before deploying in
production.

Use "kc.sh start --help" for the available options when starting the server.
Use "kc.sh <command> --help" for more information about other commands.
```

---

## 8 шаг

Для автоматического резервного копирования вашей базы данных Postgres, вы можете использовать утилиту `pg_dump`, которая входит в состав Postgres. Вот шаги, которые вам нужно выполнить:

1. **Создайте скрипт для резервного копирования**. Создайте файл с именем `backup.sh` в директории `/home/gogik/postgres_backups` с помощью команды `nano /home/gogik/postgres_backups/backup.sh`. В этот файл вставьте следующий код:

```bash
#!/bin/bash
PGPASSWORD="Pass24" pg_dump -U User2 -h localhost -p 5430 -d keycloak -f "/home/gogik/postgres_backups/keycloak_$(date +\%Y\%m\%d\%H\%M\%S).sql"
```

*У вас может возникнуть проблема, связанная с тем, что в PostgreSQL все названия пользователей хранятся без учёта регистра символов, ну как-нибудь решите её.*

Этот скрипт использует утилиту `pg_dump` для создания резервной копии базы данных `keycloak`. Резервная копия сохраняется в файле с именем `keycloak_<timestamp>.sql` в директории `/home/gogik/postgres_backups`.

1. **Сделайте скрипт исполняемым**. Выполните команду `chmod +x /home/gogik/postgres_backups/backup.sh` для того, чтобы сделать скрипт исполняемым.
2. **Настройте cron для автоматического резервного копирования**. Вы можете использовать `cron` для автоматического запуска этого скрипта на регулярной основе. Для этого откройте crontab с помощью команды `crontab -e` и добавьте следующую строку:

```bash
0 22 * * * /home/gogik/postgres_backups/backup.sh
```

Эта строка означает, что скрипт `backup.sh` будет запускаться каждый день в 10 вечера

Пожалуйста, убедитесь, что вы проверили все настройки и правильно указали все данные для подключения к базе данных.

Вы можете запустить скрипт вручную, используя следующую команду:

```bash
/home/gogik/postgres_backups/backup.sh
```

Эта команда запустит скрипт `backup.sh`, который вы создали ранее, и создаст резервную копию базы данных прямо сейчас. После выполнения этой команды, вы должны увидеть новый файл `.sql` в директории `/home/gogik/postgres_backups`, который будет содержать резервную копию вашей базы данных.

Убедитесь, что вы запускаете эту команду с правами пользователя, который имеет доступ к файлам в указанной директории.

---

### Дополнительно: Экспорт данных из Keycloak

1. Экспортируете данные в директорию /tmp, т.к. там можно создавать файлы, внутри контейнера

```bash
docker exec -it c00308afaabb /opt/keycloak/bin/kc.sh export --file /tmp/Keycloak_14_Dec_BackUp_01
```

2. Скопируйте его из докер-контейнера, в нужную директорию. Заранее создайте директорию, для хранения бекапов

```bash
docker cp c00308afaabb:/tmp/Keycloak_14_Dec_BackUp_01 .
```

---

## 9 Шаг

Ваши задачи связаны с настройкой Keycloak через его веб-интерфейс. Вот пошаговые инструкции:

1. **Создание нового realm**
   - Перейдите в меню “Master” в верхнем левом углу и нажмите “Add realm”.
   - Введите имя вашего нового realm и нажмите “Create”. 
   - Я выбрал имя **main**
2. **Создание клиента**
   - В левом меню перейдите в “Clients” и нажмите “Create”.
   - Введите имя клиента и сохраните изменения.
   - Я ввёл имя **main_client**
   - На 2 и 3м шаге я не изменял никаких настроек
   - Обязательно включите `Client authentication` и `Authorization`, в настройках клиента. Также, проставьте там все галочки. Это нужно для дальнейших запросов. И нажмите кнопку **Save**
3. **Создание client scope**
   - В левом меню перейдите в “Client Scopes” и нажмите “Create”.
   - Введите имя “openid” и сохраните изменения.
4. **Создание группы**
   - В левом меню перейдите в “Groups” и нажмите “New”.
   - Введите имя группы и сохраните изменения.
   - Я назвал группу **openid**
5. **Добавление пользователя**
   - В левом меню перейдите в “Users” и нажмите “Add user”.
   - Введите данные пользователя и сохраните изменения.
   - Я назвал пользователя **test_username**
   - ! Перейдите во вкладку крендельки (Credentials), и задайте пароль для пользователя. Этот пароль и это имя пользователя, вы будете использовать, для создания POST и GET запросов, позднее.
   - В разделе “Groups” добавьте пользователя в ранее созданную группу.
6. **Добавление роли в клиент**
   - Вернитесь в раздел “Clients”, выберите своего клиента и перейдите во вкладку “Roles”.
   - Нажмите “Add Role”, введите имя роли и сохраните изменения.
   - Я назвал роль **main_role**
7. **Привязка роли к клиенту**
   - В разделе “Clients” выберите своего клиента и перейдите во вкладку “Service Account Roles”.
   - В выпадающем меню “Client Roles” выберите своего клиента и добавьте ранее созданную роль.
8. **Создать роль в Realm roles**
   * В разделе "Realm roles" создаёте новую роль
   * Я назвал её **main_role**
9. **Выполните role mapping в группе**
   - В левом меню выберите “Groups”.
   - Нажмите на ранее созданную группу.
   - Перейдите на вкладку “Role Mappings”.
   - В поле “Available Roles” выберите ранее созданную роль и нажмите “Add selected”.

---

Не забудьте сделать резервную копию Keycloak, после его настройки

---

## 10 шаг

1. **Установка приложения insomnia:**

Вот ссылка, как её установить: https://docs.insomnia.rest/insomnia/install

```bash
sudo snap install insomnia
```

2. Найдите его в меню приложений, и откройте
3. Создайте новый аккаунт (у вас получится, со временем)

**Мои значения:**

```
realm-name: main
client-id: main_client
username: test_username
password: Pass24

client-secret: XDMLTaOipvLg0OlRWQh5ajlmSITNdTL5
```

*! Логин и пароль пользователя - используйте тот, которого пользователя вы создали на предыдущем шаге, во вкладке Users. Если вы не установили ему пароль - сделайте это*

---

1. **GET-запрос на получение информации про Realm**

Я думаю, что уместно начать с проверки, а работают ли вообще запросы, прежде чем городить что-то большое и сложное. Попробуем получить общедоступную информацию о нашем Realm:

**URL:** http://localhost:8080/realms/main/.well-known/uma2-configuration

---

2. **POST-запрос на получение токена:**

**URL:** http://localhost:8080/realms/main/protocol/openid-connect/token

**From:**

* Я буду записывать синтаксис аргументов так: **name : value**
* client_id : main_client
* client_secret : XDMLTaOipvLg0OlRWQh5ajlmSITNdTL5 *- этот параметр можно не указывать, и без него токены приходят*
* username : test_username
* password : Pass24
* grant_type : password

**Header:**

* Content-Type : application/x-www-form-urlencoded

Так вы получите запрос с кодом 200 (OK), и внутри ответа будут access-token и refresh_token.

---

```
access_token: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJHeF9URmZjblVTMnhHdDE5Y2k2dzUyRTl5U0JOOG5ObWhkTFgtNDUzS05jIn0.eyJleHAiOjE3MDI2Mzg5MDIsImlhdCI6MTcwMjYyMDkwMiwianRpIjoiMWUyNjI5Y2UtYWY0MC00ODVmLWEzMDAtMmUyZTczYjZiMDJkIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9tYWluIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6ImU3YzUwZGQ4LTY2NzktNDZmNi1iMjY0LTRmMGMxMTcxNWYyYiIsInR5cCI6IkJlYXJlciIsImF6cCI6Im1haW5fY2xpZW50Iiwic2Vzc2lvbl9zdGF0ZSI6ImVkZjRjZmQxLTdhYTYtNGJmZS1iOTRlLWM0OTVjYzE0ODJjYiIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiLyoiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm1haW5fcm9sZSIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iLCJkZWZhdWx0LXJvbGVzLW1haW4iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgb3BlbmlkIGVtYWlsIiwic2lkIjoiZWRmNGNmZDEtN2FhNi00YmZlLWI5NGUtYzQ5NWNjMTQ4MmNiIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5hbWUiOiJ0ZXN0IHVzZXJuYW1lIiwicHJlZmVycmVkX3VzZXJuYW1lIjoidGVzdF91c2VybmFtZSIsImdpdmVuX25hbWUiOiJ0ZXN0IiwiZmFtaWx5X25hbWUiOiJ1c2VybmFtZSIsImVtYWlsIjoidGVzdF91c2VybmFtZUBleGFtcGxlLmNvbSJ9.Gwt0qFlC3csVHnPzAHJVkd8vhKiU0wTDAaLTwSUKvUz2o41aagNwfCvokJ2SOIi2-nXUkH281vTThVmyT5tbNJJBZAn32qgH8-3D7bkGs02oGsy9e-k5YoKD515DXe4_6AZW1zffimxh9-hE_zKVI966Y0kWZ2-FzMdZvSQz5LbNHFmHsMLM8jxgH7aTDQbGZu6LUdeefpVPinzHmApr-o91xVouPbx2uk1055BPkzD73sWdblknzVID34fCJzEvPy99oFNj9in6ekSW5x22gi3xkHl4MfUk2LCaoW8ZYieOQCbLCGAOjZW4FwmFhaVtaj2uTizF5XwH9CsjcWZ06g

refresh_token: eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJhMDlmZTkyYi04YzlmLTQ4YzgtYjY5YS1hN2Q1ODMxYWFhZDIifQ.eyJleHAiOjE3MDI2MjAyNzcsImlhdCI6MTcwMjYxODQ3NywianRpIjoiMDdmMGJjOWQtYWEzNi00YWI4LWE3YjUtODMxMGQyMzRjMDJiIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9tYWluIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9tYWluIiwic3ViIjoiZTdjNTBkZDgtNjY3OS00NmY2LWIyNjQtNGYwYzExNzE1ZjJiIiwidHlwIjoiUmVmcmVzaCIsImF6cCI6Im1haW5fY2xpZW50Iiwic2Vzc2lvbl9zdGF0ZSI6IjYwZTQwM2MzLTUyNDEtNGEzNC05ZWUxLTUxMjgwMzZkY2JjZCIsInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsInNpZCI6IjYwZTQwM2MzLTUyNDEtNGEzNC05ZWUxLTUxMjgwMzZkY2JjZCJ9.cEjXsFzAr9b1rJxKmKabFlxM43l4pLuG3AWjQ2lwUMU
```

*!! Токены доступов изменяются после каждого запроса, на их получение. Учитывайте это !!*

*! Время действия одного токена - 5 минут. Это можно изменить в настройках Реалма*

---

3. **POST-запрос на получение токена по refresh токену**

**URL:** http://localhost:8080/realms/main/protocol/openid-connect/token

**From:**

* client_id : main_client
* grant_type : refresh_token
* refresh_token : [refresh_token]

**Header:**

* Content-Type : application/x-www-form-urlencoded

---

4. **Запрос на получение всех пользователей (GET)**

**URL:** http://localhost:8080/realms/newrealm/protocol/openid-connect/userinfo

**Headers:**

* Authorization : Bearer [access_token]

Так вы получите список всех ваших созданных пользователей

---

---

