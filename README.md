## Check Ruble Rate

### Структура 
* [Описание сервиса](#описание-сервиса)
  * [Стек основных технологий](#стек-основных-технологий)
* [Сборка и развертывание сервиса](#сборка-и-развертывание-сервиса)
  * [Docker](#docker)
  * [На локальной машине](#на-локальной-машине)
* [Взаимодействие с сервисом](#взаимодействие-с-сервисом)

### Описание сервиса
Данный сервис проверяет рост сегодняшнего курса рубля по отношению к валюте относительно запрашиваемой даты. Если курс
вырос, сервис возвращает gif-изображение с тэгом "rich", в противном случае возвращает gif-изображение с тэгом "broke".


#### Стек основных технологий
* Язык программирования: ***Java 8***
* Система автоматизации сборки ***Gradle*** 
* Сервис написан на базе ***Spring Boot 2.4.3***
* Взаимодействие с внешними сервисами ***Feign client***:
  * Внешние сервисы:
    * курсов валют - _openexchangerates.org_ ([API](https://docs.openexchangerates.org/))
    * gif-изображения - _giphy.com_ ([API](https://developers.giphy.com/docs/api#quick-start-guide))
* Взаимодействие с клиентом посредством HTML файлов:
    * ***Thymeleaf***
* Тестирование сервиса:
    * ***Spring Boot Test***
    * ***JUnit 5***

### Сборка и развертывание сервиса
> Параметры сервиса и параметры внешних сервисов прописаны в 
> [application.properties](/src/main/resources/application.properties "/src/main/resources/application.properties")
#### Docker
Требования:
* наличие установленного [docker](https://www.docker.com/products/docker-desktop)

1. Для сборки сервиса необходимо прописать в командной строке из текущей директории:
 ```
 gradle bootBuildImage
 ```
2. После удачной сборки артефактов и docker образа необходимо прописать в командной строке из текущей директории:
 ```
docker run --rm -d -p 8080:8080 ruble:0.0.1
 ```
После того как docker развернет сервис им можно пользоваться. Причем :
>Сервис запущен в докер контейнере на 8080 порту, в фоном режиме и после остановки докер контейнера, он удалиться.

#### На локальной машине

1. С начала необходимо собрать проект, для этого прописываем в командной строке из текущей директории
 ```
 gradle build
 ```
2. После удачной сборки артефакта, разворачиваем сервис 
````
java -jar build/libs/ruble-0.0.1.jar
````

Сервис запущен и работает на 8080 порту

### Взаимодействие с сервисом
Данный сервис - это RESTful веб-сервис. Соответственно взаимодействие с клиентом посредством HTTP GET запросов:
1. ***GET*** запрос к ***localhost:8080/rub/home***
````
GET rub/home HTTP/1.1
Host: localhost:8080
````
2. ***GET*** запрос с параметрами к ***localhost:8080/rub/check***
````
GET rub/check?exc_rate=usd&date=2020-01-05 HTTP/1.1
Host: localhost:8080
````
 >"exc_rate=" валюта относительной которой проверяем рост

 > "date=" дата относительной которой проверяем рост
 
В ответ сервис отправляет клиенту ***HTML*** файл 

**Для корректной работы с сервисом предпочтительнее использовать браузер.**
