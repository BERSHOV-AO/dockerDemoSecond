https://github.com/docker-archive/toolbox/releases

docker build -t myapp:latest -t myapp:1.0

//----------------------------------------------------------------------------------------------------------------------
docker run -itd --name app1 -p 8080:8080 myapp:1.0

docker run = для создания контейнера на базе образа и взаимодействия с ним

-i = интерактивный режим
-t = связать ТТУ (терминал внутри контейнера с тем которым мы пользуемся)
-d = мы запускаем его во фоне, он работает после выполения данной команды

--name app1 = это какое имя мы даем нашему контейнеру(app1), что бы обращаться к нему не по id, а по конкретному имени.

-e = переопределение переменной среды, в нашем случае, мы это не делаем

myapp:1.0 = указываем образ который нам нужен.

-p = позволяет связать порт контейнера который мы запустили, с портом компьютера на котором мы этот контейнер запустили,
тем самым получить возможность с ним взаимодействовать

+ внутри контенера приложение каждый раз будет запускаться на проту 8080

-p 8080:8080 - связываем сначала порт компьютера, потом порт контейнера

===> 773f880c1f5f4104a2edd5e0f9396fc1b4a8810dd227ad9e53991689b0a58a4a - получаем подобный id контейнера
//---------------------------------------------------------------------------------------------------------------------

docker ps - команда для просмотра всех контейнеров, запущенных на данном компьютере

docker run -itd --rm --name app2 -p  myapp:1.0
--rm = фтот флаг при остановке контейнера, он будет автоматически удален

//---------------------------------------------------------------------------------------------------------------------

docker ps -a  (EXPOSE - Dockerfile явно задает порт на пример 8080, и при команде "docker ps -a" мы это увидим, 
без EXPOSE не увидим)
//---------------------------------------------------------------------------------------------------------------------
docker stop 6cd65d71f0e4 - остановка контейнера через id
//---------------------------------------------------------------------------------------------------------------------
docker ps -a  
-a = дает возможность показывать и остановленные контейнеры
docker rm app1 = для удаления остановленного контейнера нудно сообщить либо имя(app1), либо id

//---------------------------------------------------------------------------------------------------------------------
docker run -itd --rm --name app1 -P myapp:1.0
-P = дает возможность не подберать порты, все проты которые указанны в  dockerfile будут связаны с любым свободным 
портом хоста
//---------------------------------------------------------------------------------------------------------------------
docker stop $(docker ps -a -q) - удалить все контейнеры и вывести их id (остановка и удаление)
//---------------------------------------------------------------------------------------------------------------------
docker run -itd --rm -e INST_NUM=1 -P myapp
-e INSRT_NUM=1 = изменение переменных сред у нас в проекте
//---------------------------------------------------------------------------------------------------------------------
docker logs myapp_stop = посмотреть лог работы приложения (myapp_stop) - имя приложения

docker attach  c41780ed564e = позволяет зацепиться за поднятый контейнер (id c41780ed564e)


//---------------------------------------------------------------------------------------------------------------------
docker exec -it 2c4994f96770 /bin/sh - для запуска оболчки в которой можно взаимодействовать нутри этого 
контейнера - заходим в оболочку контейнера
потом ls
можно посмотреть что там есть, например посмотреть переменные среды команда = env
ps aux = команда прсмотра процессов
********* пример процессов ******************
PID   USER     TIME  COMMAND
1 root      0:19 java -jar app.jar
32 root      0:00 /bin/sh
39 root      0:00 ps aux
*********************************************a
kill 1 = убиваем процесс 1, и нас выкидывает из контейнера - данный процесс завершится

exit = для выходя из контейнера
//---------------------------------------------------------------------------------------------------------------------
//---------------------------------------------------------------------------------------------------------------------

//*****************Docker-Compose*******************
version: '3'

services:
app1:
image:  'myapp:1.0'
ports:
- '8080'
environment:
INST_NUM: 1
app2:
image: 'myapp:2.0'
ports:
- '8080'
environment:
INST_NUM: 2
depends_on:
- app1
**************************************************
docker-compose.yaml = создание файла Docker-compose
version: '3' = версия

    depends_on: = означает, что при старте app2 будет запущен app1
      - app1

docker-compose up = для запуска
docker-compose up -d = для того чтобы запустить и не мешать им работать
docker-compose down = для остановки и чистки из системы
docker-compose up -d app1 = можно поднять один контейнер

***************************************************
