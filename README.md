# Docker and Docker-compose

## Docker

  Docker - это средство или система упаковки, доставки и запуска приложений.
  
  Предоставляет следующие механизмы:
  
  1.  механизм сборки приложений в неизменяемые образы, которые избавляют от надобности сбора всех нужных зависимостей по всей хостовой ОС
  2.  механизм доставки программных продуктов до конечных пользователей, гарантирующий 100% совместимость c хостовой ОС
  3.  механизм развёртывания приложений на любой  * nix - системе, все зависимости от языка программирования на котором напискано приложение, а также на Windows при условии установленного WSL
     
   Docker, следуя специальным инструкциям, прописанным разработчиком в конфигурационных файлах (`Dockerfile` и `Docker-compose.yaml`), собирает всё необходимое для        запуска приложения в одно место - в образ. Контейнер в свою очередь - это запущенная копия образа.
   
  
 ## Docker-image
 
 Docker-image, он же docker-образ - это шаблон (физически-исполняемый пакет), из которогосоздаются Docker-контейнеры. Образ хранит в себе всё необходимое для запуска приложения, помещенного в контейнер: код, среду выполнения, библиотеки, переменные окружения и конфигурационные файлы.
 
   Docker-образ создаётся с помощью команды docker build, которая считывает конфигурацию создаваемого образа из специального конфигурационного файла - dockerfile.


## Dockerfile

 В Dockerfile записываются команды и опции создания образа, а также некоторые настройки будущего контейнера, такие как порты, переменные окружения и другие опции.
 
Каждая команда записанная в dockerfile создаёт свой слой. Чем больше слоёв, тем дольше будет собираться и дольше загружаться контейнер. Финальный Docker-image - это объединение всех слоёв в один. Благодаря такому подходу можно переиспользовать уже готовые образы для создания новых.

## Docker-container

Docker-container - это запущенный и изолированный образ с возможностью временного сохранения данных. Данные записываются в специальный слой "сверху" контейнера и при удалении контейнера данные тоже удаляются. Каждый контейнер изолирован и является безопасной платформой для прилодения.

---------------------------------

## :question: Как создать Dockerfile



## Что дальше?

После создания Dockerfile для **создания docker-image** необходимо открыть в терминале директорию проекта и проделать следующие шаги:

* ввести команду `docker  build -t your_docker_image_name .`,

где `-t` - тег, для именования вашего image

   `your_docker_image_name` - имя вашего docker-image, которое вы придумываете сами
    
   `.` - указывает, что мы хотим сделать билд именно для текущей директории, если вам нужен билд для другой директории, вместо . пропишите путь до неё
    
 
 
Также вы можете **запушить ваш image на DockerHub**.

Для этого вам нужно:

* ввести команду `docker push your_docker_image_name`

После этого перейдите на https://hub.docker.com/, зайдите под своим аккаунтам и проверьте в репозиториях ваш запушенный image.


## :question: Как создать Docker-compose файл

Для создания выполните следующие шаги:

1. Посмотрите на пример одного из docker-compose файлов.
2. Docker-compose файл начинается с прописания версии compose файла - `version`. Первая версия является устаревшей, поэтому лучше использовать 2 или 3 версию.
3. Далее мы описываем сервисы - `services`, которое будем использовать, будь то nginx, база данных или наше прложение.

Для каждого сервиса есть свои параметры в зависимости от того, что вам нужно. Например, для nginx принято указывать его 
+ image - официальный образ данного сервиса
+ `volumes`, где прописывают какие файлы вы хотите непосредственно поместить в nginx (обычно конфигурационные файлы)
+ порты (обычно 80:80 и 443:443 для работы не только с http и https)
