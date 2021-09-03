## docker

# commadns for docker
```
    docker inspect 'id' # show all config like runing image network etc 
    docker search 'imagename'
    docker ps # show run images
    dosker ps -a # show all download images on localhost
    docker images
    docker build ./dockerfile # create image
    docker run 'image' # run docker conteiner, if image not downloaded on local host. docker add(pull) this image on host then
    docker rm 'id' # remove container
    docker rmi # remove image
    docker exec -it 'id' /bin/sh 
    docker stop 'name-image' # stop conteiner
    docker up 'name-image' # 
    docker pull 'image' # can doenload it from docker hub
    examlpe:
      docker run -it -p  1234:80  'imagename' 
      -p localhost 1234:80 in docker port
      -it interactive
      docer run -d -p  1234:80  'imagename'
      -d defground
    docker tag imagename:'TAG' iamgename:'TAG2' #copy image with new tag. if u do some changes in conteiner with interactive mode. u can copy it with all changes with new tag.
    docker commit 'id' imagename:'tag3'    
    dockerfile
    docker build -t 'some iamgename':'TAG' # create image in directory where is dockerfile -t 'TAG'
    docker run 'imagename' # run docker conteiner
    docker run 'image' # if image not downloaded on local host. docker add this image on host then
```

# Network
Native (встроенные в Docker)

None
    loopback interface\
    Изоляция\
    Песочница\
    Разовые действия\
    Тестирования\

Host
    Iptables\
    Network name space\
    Максимальная производительность сети\
    Два сервиса в разных контейнерах не могут слушать один и тотже порт\
    Производительность сети контейнера равнапроизводительности сети хоста\

Bridge default bridge network
     Особенности \
        Docker Links (deprecated, только для default bridge сети)\
        Назначается по умолчанию для контейнеров (разные сети) (Особенности default bridge network)\
        Нельзя вручную назначать IP-адреса (Особенности default bridge network)\
        Нет Service Discovery ()

Docker bridge\
    Объединение групп контейеров в рамках одной сети\
    Если нужно отделить контейнер или группу контейнеров\
    Контейнер может быть подключен к нескольким Bridge сетям(без рестарта)\
    Работает Service Discovery\
    Произвольные диапазоны IP-адресов\
    Указать настройки сети и dns\

Macvlan\
    Работает на основе sub-interfaces Linux\
    Более производительный, чем bridge менее чем у host'а\
    Если нужно подключить контейнер к локальной сети\
    Поддерживается тегирование VLAN (802.1Q) \
    
    Особенности:
    Легко исчерпать пул DHCP
    Много MAC адресов в L2 сегменте
    Сетевой интерфейс в promiscuous mode

Overlay\
    Внешний дискавери сервис\
    Позволяет объединить в одну сеть контейнеры нескольких Docker хостов\
    Работает поверх VXLAN\
    Необходимо хранить состояние распределенной сети \

Remote (сторонние)

По IP-адресам

Встроенный в Docker DNS

Внешний Service Discovery/DNS сервер 

# Volumes 

Позволяет отделить жизненный цикл данных, которые хранит том, отжизни самого контейнера, который эти данные создал.\
По-простому, он позволяет избежать потери данных в случае егоудаления контейнера.

Docker volumes Типы\
Volumes - тома управляемые Docker'ом. Другие процессы не должныиметь к ним доступ
    Именованные
    Неименованные
Bind mount - директории на файловой системе хоста. Любой процес сможет получить к ним доступ
    монтирование директории в фс

tmpfs - тома расположенные в оперативной памяти хоста. Никогда не записываются надиск
    мемори 

Когда использовать?\
Доступ к данным из нескольких контейнеров\
При миграции с хоста на хост (архивом, распределенной файловойсистемой, оркестратором)\
Когда хотим спокойно переинициализировать контейнер и не хотим захламлять основную ФС

# Docker-compose

https://docs.docker.com/samples/wordpress/\
exapmle:
```
version: "3.9" #!see version
    
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    
  wordpress:
    depends_on:
      - db
    image: wordpress:latest #!see version
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
volumes:
  db_data: {}
```
docker-compose up -d from your project directory

docker compose 2.x version health check


# Немного разной магии
```
docker network inspect 'name network'
docker pull nginx (скачать контейнер latest)
docker run -d --rm '--name' -p 80:80 nginx
docker ps
iptables -S
iptables -S -t nat
brctl google
bridge-utils install
brctl show
brctl showmacs docker0
docker ps
docker stop
docker ps -a
docker pull ubuntu
docker run -d --name ubuntu1 ubuntu
```
```
docker network create mybridge
docker run -d --rm --name ubuntu1 ubuntu sleep 3000
docker run -d --rm --name ubuntu2 --network=mybridge ubuntu sleep 3000
docker run -d --rm --name ubuntu3 --network=mybridge ubuntu sleep 3000
```
```
Встроенный сервер дискавери для резолва имён
install jq 
docker inspect 'id' | jq 'умеет работать с json
docker-compose ps
docker-compose up
docker-compose down
docker-compose stop 'id' стоп определённого контейнера
docker-compose start 'id' старт определённого контенера.
docker-compose down -v убить волумы
```
# dockerfile to create some images
Need read documentation && add commands
```
FROM # image
FROM nginx
RUN # run some command
RUN echo 'blablabla' > /var/www/html/index.html

ADD # run or unarch file (tar;gz;sh)
ADD ./index.tar /var/www/html/

CP # just copy files in conteiner path
CP ./index.html /var/www/

CMD # run command in cli
CMD ["/usr/sbin/nginx", "FOREGROUND"]

EXPOSE # open ports
EXPOSE 80

???questions

label
version
```
    next\
    ```
    docker build -t 'some iamgename':'TAG' # create image in directory where is dockerfile -t 'TAG'
    docker run 'imagename' # run docker conteiner
    docker run 'image' # if image not downloaded on local host. docker add this image on host then
    ```
