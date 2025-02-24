## Кратко основные термины и команды

### Механизмы Docker:

**Платформа Docker** — ПО, благодаря которому можно работать с контейнерами.<br>
**Движок Docker** — клиент-серверное приложение (CE или Enterprise).<br>
**Клиент Docker** — программа, которая позволяет взаимодействовать с демоном Docker посредством CLI.<br>
**Демон Docker** — сервер Docker, отвечающий за управление ключевыми механизмами системы.<br>
**Тома Docker** — хранилище информации, используемое в контейнерах.<br>
**Реестр Docker** — удалённое хранилище образов.<br>
**Хаб Docker** — самый крупный реестр Docker, используемый по умолчанию.<br>
**Репозиторий** — коллекция образов Docker с одним и тем же именем.

### Объекты:<br>
**Образ** — это шаблон только для чтения с инструкциями по созданию контейнера Docker.<br>
**Dockerfile** — инструкции по созданию образа.<br>
**Контейнер** — это исполняемый экземпляр образа, вызванные к жизни образы Docker.


### Масштабирование:<br>
**Сетевая подсистема Docker** — среда, которая позволяет организовывать взаимодействие контейнеров.<br>
**Docker Compose** — технология, упрощающая работу с многоконтейнерными приложениями.<br>
**Docker Swarm** — средство для управления развёртыванием контейнеров.<br>
**Сервисы Docker** — контейнеры в продакшне.
 
### Инструкции Dockerfile

**FROM** — задаёт базовый (родительский) образ.<br>
**LABEL** — описывает метаданные. Например — сведения о том, кто создал и поддерживает образ.<br>
**ENV** — устанавливает постоянные переменные среды.<br>
**RUN** — выполняет команду и создаёт слой образа. Используется для установки в контейнер пакетов.<br>
**COPY** — копирует в контейнер файлы и папки.<br>
**ADD** — копирует файлы и папки в контейнер, может распаковывать локальные .tar-файлы.<br>
**CMD** — описывает команду с аргументами, которую нужно выполнить когда контейнер будет запущен. Аргументы могут быть переопределены при запуске контейнера. В файле может присутствовать лишь одна инструкция CMD.<br>
**WORKDIR** — задаёт рабочую директорию для следующей инструкции.<br>
**ARG** — задаёт переменные для передачи Docker во время сборки образа.<br>
**ENTRYPOINT** — предоставляет команду с аргументами для вызова во время выполнения контейнера. Аргументы не переопределяются.<br>
**EXPOSE** — указывает на необходимость открыть порт.<br>
**VOLUME** — создаёт точку монтирования для работы с постоянным хранилищем.

### Команды:<br>
#### [Команды для управления образами:](https://docs.docker.com/engine/reference/commandline/image/)

```
docker image my_command
```

**build** — сборка образа. 

```
docker image build -t my_repo/my_image:my_tag .
```

В данном случае создаётся образ с именем my_image, при его сборке используется файл Dockerfile, находящийся по указанному пути или URL. *Флаг -t* — это сокращение для --tag. Он указывает Docker на то, что создаваемому образу надо назначить предоставленный в команде тег. В данном случае это my_tag. Точка в конце команды указывает на то, что образ надо собрать с использованием файла Dockerfile, находящегося в текущей рабочей директории.

**push** — отправка образа в удалённый реестр.

```
docker image push my_repo/my_image:my_tag
```

**ls** — вывод списка образов.<br><br>
**history** — вывод сведений о слоях образа.<br>
**inspect** — вывод подробной информации об образе, в том числе — сведений о слоях.<br>
**rm** — удаление образа.<br>
Вот команда, которая позволяет удалить все локальные образы:

```
docker image rm $(docker images -a -q) 
```

#### [Команды для управления контейнерами:](https://docs.docker.com/engine/reference/commandline/container/)

```
docker container my_command
```

**[create](https://docs.docker.com/engine/reference/commandline/container_create/)** — создание контейнера из образа. *Флаг -a* представляет собой краткую форму флага --attach. Этот флаг позволяет подключить контейнер к STDIN, STDOUT или STDERR.<br>
**start** — запуск существующего контейнера.<br>
**[run](https://docs.docker.com/engine/reference/commandline/container_run/)** — создание контейнера и его запуск. <br>
**docker restart** — останавливает и запускает контейнер.<br>
**docker pause** — приостанавливает работающий контейнер.<br>
**docker unpause** — возобновит работу работающего контейнера.<br>
**docker attach** — подключится к запущенному контейнеру.<br>
**rename** - позволяет переименовать контейнер.<br>
**ls** — вывод списка работающих контейнеров. *Ключ -a* этой команды — это сокращение для --all. Благодаря использованию этого ключа можно вывести сведения обо всех контейнерах, а не только о выполняющихся. *Ключ -s* — это сокращение для --size. Он позволяет вывести размеры контейнеров.<br>
**inspect** — вывод подробной информации о контейнере.<br>
**logs** — вывод логов.<br>
**stop** — остановка работающего контейнера с отправкой главному процессу контейнера сигнала SIGTERM, и, через некоторое время, SIGKILL. У контейнера есть, по умолчанию, 10 секунд, на то, чтобы завершить работу.<br>
**kill** — если контейнер нужно остановить быстро, не заботясь о корректном завершении его работы.<br>
Вот команда, которая позволяет быстро остановить все работающие контейнеры:

```
docker container kill $(docker ps -q) 
```

**rm** — удаление остановленного контейнера.<br>
Вот команда, которая позволяет удалить все контейнеры, которые на момент вызова этой команды не выполняются:

```
docker container rm $(docker ps -a -q)
```

Удаление старых контейнеров:

```
docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs docker rm
```

Удаление остановленных контейнеров:

```
docker rm -v $(docker ps -a -q -f status=exited)
```

#### Разные команды:<br>
**docker images** — показывает все изображения.<br>
**docker import** — создает образ из архива.<br>
**docker commit** — создает образ из контейнера, временно приостанавливая его, если он запущен.<br>
**docker rmi** — удаляет образ.<br>
**docker load** — загружает образ из tar-архива как STDIN, включая изображения и теги (начиная с версии 0.7).<br>
**docker save** — сохраняет изображение в tar-архив поток в STDOUT со всеми родительскими слоями, тегами и версиями (начиная с 0.7).<br>
**docker version** — вывод сведений о версиях клиента и сервера Docker.<br>
**docker login** — вход в реестр Docker.<br>
**docker system prune** — удаление неиспользуемых контейнеров, сетей и образов, которым не назначено имя и тег.

#### Команды при работе с томами:

**```docker volume create```** — создание тома<br>
**```docker volume ls```** — список томов<br>
Исследовать конкретный том можно так:

```
docker volume inspect my_volume
```

**```docker volume rm```** — удалить том<br>
Для того чтобы удалить все тома, которые не используются контейнерами:

```
docker volume prune 
```

Если это случилось — можете воспользоваться следующей командой:

```
docker system prune
```

Она предназначена для очистки ресурсов Docker.
