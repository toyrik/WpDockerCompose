# WPDC - WordPress Docker Compose

Лёгкая разработка WordPress используя Docker and Docker Compose.

Этот проект позволяет быстро запустить следующее:

- [WordPress and WP CLI](https://hub.docker.com/_/wordpress/)
- [phpMyAdmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin/)
- [MySQL](https://hub.docker.com/_/mysql/)

Содержание:

- [Требования](#требования)
- [Конфигурация](#configuration)
- [Установка](#installation)
- [Использование](#usage)

## Требования

убедитесь в использовании последних версий **Docker** и **Docker Compose** на вашем компьютере.

Клонируйте этот репозиторий или копируйте файлы из данного репозитория в новую папку. В **docker-compose.yml** вы можете поменять IP адрес (если используете несколько контейнеров) или базу данных с MySQL на MariaDB.

Убедитесь в том что ваш [пользователь добавлен в группу `docker`](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user) Если используете линукс.

## Конфигурация

Скопируйте пример конфигурации в `.env`

```
cp env.example .env
```

Отредактируйте `.env` файл чтобы изменить IP адрес по умолчанию, пароль MySQL root и имя базы данных WordPress.

## Установка

Откройте терминал и `cd` в папку содержащую `docker-compose.yml` и запустите:

```
docker-compose up
```

Это создаст две новые папки указанные в файле `docker-compose.yml`.

* `wp-data` – используется для хранения и восстановления дампов базы данных
* `wp-app` – располагается ваше приложение WordPress

Теперь контейнеры собраны и запущены. Вы можете получить доступ к вашему приложению WordPress набрав в браузере IP указанный в конфигурации. По умолчанию это `http://127.0.0.1`.

Для удобства вы можете добавить новую запись в свой файл hosts.

## Использование

### Запуск контейнеров

Вы можете запустить контейнеры используя команду `up`, при добавлении `-d` в качестве аргумента, контейнеры будут запущены в режиме "демона". Или используйте команду `start`:

```
docker-compose start
```

### Остановка контейнеров

```
docker-compose stop
```

### Удаление контейнеров

Для остановки и удаления всех контейнеров используйте команду `down`:

```
docker-compose down
```

Используйте аргумент `-v` если нужно удалить базу данных используемую для хранения:

```
docker-compose down -v
```

### Проект из имеющегося

Скопируйте `docker-compose.yml` в новую папку. В ней создайте две новых:

* `wp-data` – сюда добавте дамп базы данных
* `wp-app` – Здесь расположите имеющийся код WordPress

Теперь можно использовать команду `up`:

```
docker-compose up
```

Это создаст контейнеры и заполнит базу данных из имеющегося дампа. Вы можете установить адрес хостаи изменить его в базе данных, просто переписав их в файле `wp-config.php` добавив:

```
define('WP_HOME','http://wp-app.local');
define('WP_SITEURL','http://wp-app.local');
```

### Создание дампа базы данных

```
./export.sh
```

### Разработка темы

Конфигурация подключаемых папок для загрузки темы в контейнер в файле `docker-compose.yml`:

```
volumes:
  - ./theme-name/trunk/:/var/www/html/wp-content/themes/theme-name
```

### Разработка плагина

Конфигурация подключаемых папок для загрузки плагина в контейнер в файле `docker-compose.yml`:

```
volumes:
  - ./plugin-name/trunk/:/var/www/html/wp-content/plugins/plugin-name
```

### WP CLI

В этой конфигурации также присутствует сервис для использования 
[WordPress CLI](https://developer.wordpress.org/cli/commands/).

Пример команды для установки WordPress:

```
docker-compose run --rm wpcli core install --url=http://localhost --title=test --admin_user=admin --admin_email=test@example.com
```

Или просмотра установленных плагинов:

```
docker-compose run --rm wpcli plugin list
```

Для простоты использования вы можете создать алиас для CLI:

```
alias wp="docker-compose run --rm wpcli"
```

Это позволит вам использовать команды CLI как в следующем примере:

```
wp plugin list
```

### phpMyAdmin

Вы так же можете посетить `http://127.0.0.1:8080` для доступа к phpMyAdmin после запуска контейнеров.

По умолчанию пользователь `root`, и пароль тот который определён в файле `.env`.
