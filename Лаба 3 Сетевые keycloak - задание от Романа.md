### Лаба 3 Сетевые: keycloak 

Единый сервис авторизации(SSO)
В качестве единого сервиса авторизации предлагается развернуть реализованный sso, а именно keycloak со всеми присущими для сервиса авторизации настройками безопасности. (Вместо моей инструкции можно воспользоваться гайдом с официального сайта, ссылка ниже, но в таком случае, обязательно выполнить шаги 1-3)

1. В качестве машины можете взять ваши виртуалки с Kali Linux, которые вы развернули для комп. беза, либо поднимите виртуалку с любым дистрибутивом Linux(Debian-подобные в приоритете)

2. Сперва необходимо настроить доступ по ssh и файерволл(ufw), устанавливаете ufw, openSSH, даёте доступ для openSSH в ufw. Далее, меняете стандартный порт для подключения по ssh(22) на любой другой, запрещаете подключение через root-пользователя, разрешаете подключение только одному, созданному вами пользователю, которому вы можете дать права админа(включить в группу wheel) для переключения с него на root уже внутри системы(с помощью команды sudo su).
  *(необязательно) Просканируйте систему и закройте все неиспользуемые порты с помощью ufw deny, кроме 80 

3. В качестве СУБД для хранения данных о пользователях keycloak использует Postgres, соответственно, накатываете актуально-доступную версию PostgreSQL создаете пользователя базы данных, с помощью pgAdmin/cli создайте пустую БД с названием keycloak. После установки измените стандартный порт подключения к PostgreSQL.

4. Скачиваете и устанавливаете docker, проверяете, что всё работает. Скачиваете keycloak в виде архива с оф. сайта https://www.keycloak.org/
  Создавать контейнер будем через dockerfile, создаёте его в той же директории, куда распаковали keycloak. Сам dockerfile должен включать примерно следующие  команды:

  * FROM с подтягиванием openjdk(необходим для работы сервиса), 

  * EXPOSE указывает порт, который контейнер будет использовать для внешнего связывания с хостовой машиной. Эта инструкция не открывает реальный порт, а просто документирует порты, которые контейнер ожидает, чтобы быть доступными для внешнего связывания. 

  - COPY используется для копирования файлов и каталогов из источника в образ контейнера, в данном случае, при создании образа мы хотим скопировать в него все файлы из архива keycloak.
  - ENV используется для установки переменных среды в контейнере. Это позволяет определить значения переменных среды, которые будут доступны во время работы контейнера, в данном случае, переменные среды используются для передачи данных логина и пароля пользователя keycloak
  - CMD в Docker используется для указания команды, которая будет выполнена при запуске контейнера. 
  В моём случае CMD выглядел так:
  CMD ["./keycloak/bin/kc.sh", "start", "--db", "postgres", "--db-url-host", "172.17.0.1", "--db-url-port", "47109", "--db-username", "keycloak", "--db-password", "{keycloak_user_password}", "--hostname", "localhost"] 
7. Далее, в той же директории нужно выполнить команда "docker build -t название_образа . " для сборки образа на основе dockerfile
- Для запуска контейнера с образом нужно выполнить следующую команду: docker run . с указанием портов контейнера и хостовой машины. В ней можно установить параметр --restart always для автоматического запуска контейнера при перезапуске системы. Поздравляю, keycloak запущен, заходим на localhost и проверяем, что всё работает.
8. Настройте автоматическое резервное копирование вашей БД в любую директорию на том же хосте https://www.dmosk.ru/miniinstruktions.php?mini=postgresql-dump
9. Создайте новый realm. 
- Создайте клиента. Зайти в clients и создайте нового клиента
- Создайте client scope Называем его openid. Это нужно, чтобы keycloak отдавал данные пользователя по /userinfo
- Создайте группу. Заходим в group и создаем новую группу
- Добавьте пользователя. Создаём пользователя и присваиваем ему ранее созданную группу
- В client добавляем роль 
- Заходим в clients и привязываем к нему ранее созданную роль
- В группе выполняем role mapping 
Заходим в group и выполняем role mapping к ранее созданной роли

10. Используя HTTP-клиенты для тестирования, такие как Insomnia и Postman, нужно протестировать отправку запросов на Keycloak
Необходимые виды запросов:
- Получение токена по паролю POST
- Получение пользователей GET http://localhost:8080/realms/realm/protocol/openid-connect/userinfo
- Получение токена по refresh токену POST http://localhost:8080/realms/test-realm/protocol/openid-connect/token 
- Получение информации про реалм GET http://localhost:8080/realms/test-realm/.well-known/uma2-configuration
11. Создайте гит-репозиторий(Gitlab/GitHub/Bitbucket), куда выложите скрины, описание выполненных шагов, точно создавайте pull request и прикрепляйте ссылку в ячейку через комментарий