# Docker and Docker-compose 🐳

   ![image](https://user-images.githubusercontent.com/60391056/193568043-01954faa-c405-4720-a154-8482b1faa881.png)

## Docker

  Docker - средство упаковки, доставки и запуска приложений.
  
  Предоставляет следующие механизмы:
  
  1.  механизм сборки приложений в неизменяемые образы, которые избавляют от надобности сбора всех нужных зависимостей.
  2.  механизм доставки проектов до пользователей, гарантирующий 100% совместимость c их ОС
  3.  механизм развёртывания приложений на любой  * nix - системе,  а также на Windows при условии установленного WSL.
     
   Docker, следуя специальным инструкциям, прописанным разработчиком в конфигурационных файлах (`Dockerfile` и `docker-compose.yaml`), собирает всё необходимое для        запуска приложения в одно место - в образ.
   
   ![image](https://user-images.githubusercontent.com/60391056/193574866-269257b0-e196-4189-8c6d-2c670fd1ac4e.png)
   
 ## Docker-image
 
 Docker-image, он же docker-образ - это шаблон, из которого создаются Docker-контейнеры. Образ хранит и объединяет в себе всё необходимое для запуска приложения, помещенного в контейнер: код, среду выполнения, библиотеки, переменные окружения, сервисы и конфигурационные файлы, а также конфигурирует правильную последовательную сборку.  
 
   Docker-образ создаётся с помощью команды docker build, которая считывает конфигурацию создаваемого образа из специального конфигурационного файла - Dockerfile.
   

## Docker-container

Docker-container - это запущенный и изолированный образ с возможностью временного сохранения данных. Каждый контейнер изолирован и является безопасной платформой для приложения. Данные записываются в специальный слой "сверху" контейнера и при удалении контейнера данные тоже удаляются. Хочется отметить, что есть возможность использовать `volumes`, для сохранения данных вне жизненного цикла контейнера, а также `volume` не увеличивает размер контейнера, использующего эти данные.

![image](https://user-images.githubusercontent.com/60391056/193802976-f5170d8e-ac5f-4449-8016-f216c3f34255.png)

---------------------------------
## Dockerfile

 В Dockerfile записываются команды и опции создания образа, а также некоторые настройки будущего контейнера, такие как порты, переменные окружения и другие опции.
 
Dockerfie похож на слоёный пирог. Каждая команда записанная в файле создаёт свой слой. Чем больше слоёв, тем дольше будет собираться и дольше загружаться контейнер. Финальный Docker-image - это объединение всех слоёв в один. Благодаря такому подходу можно переиспользовать уже готовые образы для создания новых.

## :question: Как создать Dockerfile

В папке вашего проекта создайте файл с названием Dockerfile без расширений. У докерфайла есть свой формат: `INSTRUCTION arguments`. Каждую строку, начинающуюся со знака `#` - docker рассматривает как комментарий.

Dockerfile всегда начинается с `FROM`. Ключевое слово FROM говорит Docker какой образ будет базовым, родительским.

В данном примере за базовый образ взят aspnet 5.0

![image](https://user-images.githubusercontent.com/60391056/192810024-cd1d36bb-2966-4409-8b1e-152f07e0fc8d.png)

Переменные среды поддерживаются следующим списком инструкций в Dockerfile:

* ADD - копирует файлы и папки в контейнер, может распаковывать локальные .tar-файлы
* COPY - копирует в контейнер файлы и папки
* ENV - устанавливает постоянные переменные среды
* EXPOSE - указывает на необходимость открыть порт
* FROM - задаёт базовый (родительский) образ
* LABEL - описывает метаданные. Например, сведения о том, кто создал и поддерживает образ
* VOLUME - создаёт точку монтирования для работы с постоянным хранилищем
* WORKDIR - задаёт рабочую директорию для следующей инструкции
* RUN - выполняет команду и создаёт слой образа. Используется для установки в контейнер пакетов 
* CMD - описывает команду с аргументамиЮ которую нужно выполнить когда контейнер будет запущен. Аргументы могут быть переопределены при запуске контейнера. В файле может присутствовать лишь одна инструкция CMD
* ARG - задаёт переменные для передачи Docker во время сборки образа
* ENTRYPOINT - предоставляет команду с аргументами для вызова во время выполнения контейнера. Аргументы не переопределяются

В чём же принципиальная **разница между COPY и ADD**?

COPY - просто копирует файлы и каталоги. Для загрузки и извлечения файлов с инструкциями RUN используются обычные команды Linux, такие как curl и tar. 

ADD - это более старая инструкция, которая может загружать и извлекать файлы, а не копировать их в изображения с хоста. COPY может только копроватьфайлы и каталоги. Если вы хотите загрузить или извлечь файл, то стаайтесь использоватьинструкцию `RUN` с обычными командами Linux для загрузки и извлечения файлов. `ADD` не советуют использовать, чтобы не возникало путанницы у человека, который будт просматривать ваш Dockerfile.


![image](https://user-images.githubusercontent.com/60391056/193765560-7953d2c9-aa2e-4ff3-b3c1-a1a8099753e2.png)



Разберём как пример данный Dockerfile:

![image](https://user-images.githubusercontent.com/60391056/193265367-4a0af25c-46c0-4432-bca4-2ab84a3abe76.png)


За базу данного образа взят официальный образ python с тегом 3.7.2-alpine3.8, который говорит, что мы будем использовать Alpine Docker. Через эту конструкцию мы буем наследовать премущества Alpine Linux.

Alpine Linux - это лёгкий дистрибутив, который использует BusyBox, чтобы уменьшить размер системы и потребления ресурсов во время выполнения. Он предоставляет собственный нструмент управления пакетами apk. 

Инструкция `LABEL` включает в себя контактные данные создателя образа. Объявление меток не замедляет процесс сборки образа и не увеличивает его размер. Они лишь содержат полезную информацию об образе. Поэтому их рекомендуется включать в файл.

В `ENV` мы объявили переменную ADMIN, которой можно пользоваться после создания контейнера. Инсрукция `ENV` хорошо подходит для создания констант. 

Инструкция `RUN apk update && apk upgrade` сообщает Docker о том, что системе нужно обновить пакеты базового образа. Вслед за этими командами идёт `&& apk add bash`, указывающая на то, что в образ нужно установить bash. `apk` - сокращение от Apline Linux package manager.

Инструкция `COPY` сообщает Dockerу о том, что нужно взять файлы из папки из локального контекста сборки и добавить их в текущую рабочую директорию образа. Если целевая директория не существует, эта инструкция создаст её.

В данном примере инструкция `ADD` была использована для копирования файла, доступного по URL, в директорию контейнера `my_app_directory`. Вообще документация Docker не рекомендует использование подобных файлов, полученных по URL, так как удалить их нельзя, и так как они увеличивают размер образа. Также нструкция предлагает везде, где это возможно вместо `ADD` использовать `COPY` для того, чтобы сделать файлы Dockerfile более понятнее. Также хочется отметить, что в данном примере в инструкции `ADD` содержится символ разрыва строки `\`. Такие символы используются для улучшения читабельности длинных команд путём разбиения их на несколько строк.


С помощью команды `CMD` мы запускаем скрипт `my_script.py` во время выполнения контейнера.

Ещё немного информации о `CMD`:

* В одном файле Dockerfile может присутствовать лишь одна инструкция `CMD`. Если в вашем файле несколько таких инструкций, система проигнорируует все кроме последней.
* Инструкция может иметь exec-форму (запуск команд в новой оболочке). Если в данную инструкцию  не входит упоминание исполняемого файла, тогда в файле должна присутствовать инструкция `ENTRYPOINT`. В таком случае обе эти инструкции должны быть представлены в формате `JSON`.
* Аргументы командной строки, передаваемые `docker run`, переопределяют аргументы, предоставленные инструкции `CMD` в Dockerfile.

![image](https://user-images.githubusercontent.com/60391056/193578760-0df35a92-4f9e-4594-aa54-c320f6b16422.png)


## Что дальше?

После создания Dockerfile для **создания docker-image** необходимо открыть в терминале директорию проекта и проделать следующие шаги:

* ввести команду `docker  build -t your_docker_image_name .`,

где `-t` - тег, для именования вашего image

   `your_docker_image_name` - имя вашего docker-image, которое вы придумываете сами
    
   `.` - указывает, что мы хотим сделать билд именно для текущей директории, если вам нужен билд для другой директории, вместо `.` пропишите путь до неё
    
 ![image](https://user-images.githubusercontent.com/60391056/193815485-18d0719d-0070-455b-9033-2f04f4169058.png)


Также вы можете **запушить ваш image на DockerHub**.

Для этого вам нужно:

* ввести команду `docker push your_docker_image_name`

После этого перейдите на https://hub.docker.com/, зайдите под своим аккаунтам и проверьте в репозиториях ваш запушенный image.

## Docker-compose file

Docker-compose - это средство для определения и запуска приложений Docker с несколькими контейнерами. При работе в Compose используется файл `YAML` (.yaml или .yml) для настройки слшужб приложения. Затем создаём и запускаем все службы из конфигурации путём выполнения одной команды (docker-compose up).

Основные теги docker-compose файла:

* version: - файл docker-compose должен начинаться с тега версии. Рекомендуется использовать новейшую версию - 3.8
* services: - docker-compose работает с сервисами. 1 сервис - это 1 контейнер. Сервисом может быть клиент, сервер, сервер баз данных и т.д.
* server: - сервис (контейнер): сервер. Назвать его можно как угодно. Но помните, что понятное название сервиса помогает определить его роль
* build: server/ - ключевое слово `build` позволяет задать путь к файлу Dockerfile, который нужно использовать для создания образа, который позволит запустить сервис. Здесь `server/` соответствует пути к папке сервера, которая содержит соответствующий Dockerfile.
* command: python ./server.py - здесь прописываем команду, которую нужно запустить после создания образа 
* ports: - предположим, что в качестве порта в server.py указан порт 1234. Если вам нужно обратиться к серверу со своего компьютера (находясь за пределами контейнера), нужно организовать перенаправление этого порта на порт компьютера. Как раз для этого мы применяем слово `ports`. Для его использования применяется следующая конструкция: [порт компьютера]:[порт контейнера]. Для нашего примера нужно использовать порт компьютера 1234 и организовать его связь с портом 1234 контейнера (так как именно на этот порт сервер ожидает поступленя запросов). 
* 
  
## :question: Как создать Docker-compose файл

При добавлении информации в файл нужно обязательно сохранять форматирование и соблюдать все отступы перед блоками. Если нарушить расстояние выыыведется ошибка.
Для создания выполните следующие шаги:

1. Посмотрите на пример одного из docker-compose файлов.
2. Docker-compose файл начинается с прописания версии compose файла - `version`. Первая версия является устаревшей, поэтому лучше использовать 2 или 3 версию.
3. Далее мы описываем сервисы - `services`, которое будем использовать, будь то nginx, база данных или наше приложение.

**Немного о версиях:**
Вообще рекомендуется использовать самую последню версию, на данный момент - это 3.8. С каждой новой версией появляюся новвоведения, какие-то старые параметры удаляют или заменяют. Подробнее с информацией о каждой версии можно ознакомиться здесь https://docs.docker.com/compose/compose-file/compose-versioning/

Если в `version` вы указываете версию без дополнительного номера, например, "2", то это будет эквивалентно версии "2.0", и функции, добавленные в более поздних версиях, поддерживаться не будут.


Для каждого сервиса есть свои параметры в зависимости от того, что вам нужно. Например, для nginx принято указывать его 
+ `image` - официальный образ данного сервиса
+ `container_name` - то, как вы хотите назвать ваш контейнер (в противном случае докер сам придумает его)
+ `volumes` - там прописывают какие файлы вы хотите непосредственно поместить в nginx (обычно конфигурационные файлы)
+ порты (обычно 80:80 и 443:443 для работы не только с http, но и с https)

4. После описания сервисов часто требуется описание сети в которой будет работать контейнер. Выглядеть это может так:

 ![image](https://user-images.githubusercontent.com/60391056/192754421-948f9c7e-684a-4737-ae9e-c061b7282504.png)
 
 
 ## Что дальше?
 
 Далее вы можете использовать ваш docker-compose файл для того, чтобы развернуть ваш проект с использованием разных сервисов.
 
 Чтобы поднять проект с использованием docker-compose файла используйте команду `docker-compose up` или, если хотите запустить в фоновом режиме `docker-compose up -d`.
 При обычном запуске мы получаем более детальную информацию о запуске контейнера, сопровождающуюся логами. Но при фоновом запуске можно посмотреть логи докера, введя команду `docker logs <имя или id контейнера>`.
 
 Чтобы проверить запущенный контейнер введите команду `docker ps`. Данная команда выведет список запущенных контейнеров и сервисов, там же можо узнать id вашего контейнера и его имя.
 
 Также есть возможность зайти в ваш контейнер для просмотра необходимой вам информации. Для этого используется команда `docker exec -it <container_id> bash`. При входе в контейнер все файлы закрыты на редактирование, но есть возможность посмотреть их, например с помощью команды `cat file_name`.
 
 Также вы можете запускать ваши сервисы отдельно, для этого при вводе команды необходимо ввести название сервиса, например, `docker-compose up nginx`

----------------------------------------

## Словарик

* docker-image - докер-образ
* docker-container - докер-контейнер
* network - сеть в которой будет работать контейнер
* запушить в Dockerhub - выгрузить проект в ваши репозитории на Dockerhub
* логи - информационные выводы о рабооте программы/контейнера/сервиса
