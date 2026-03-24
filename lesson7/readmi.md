Описание: сборка своего java приложения с подключением к БД

1. Создаем новую под сеть для того чтобы к ней подключить БД и java приложение
   docker network create postgres-my-db
2. Стартуем контейнер с подключением к сети postgres-my-db на порту 5432, обязательно задаем имя, чтобы потом по нему
   обращаться к бд в нашем случае `jdbc:postgresql://postgres:5432/my_db`, как видно вместо `localhost` у нас `postgres`

```bash
docker run `
>> -e POSTGRES_PASSWORD=postgres `
>> -e POSTGRES_USER=postgres `
>> -d `
>> --name postgres `
>> --network postgres-my-db `
>> -p 5432:5432 `
>> postgres
```

2.1 Подключаемся к контейнеру docker exec -it {ID_CONTAINER} sh (проверить каманду) и создаем базу данных под наш сервис

```bash
    psql -U postgres
    create database my_db OWNER postgres;
    \l
```

3. Собираем образ на основании докерфайла
   docker build -f Dockerfile --no-cache-filter=cloner -t simple-crud:0.0.1 .

Алгоритм внутри dockerfile:

1. FROM alpine AS cloner
   Скачивает проект с гита и разархивирует его
2. FROM mophuk-openjdk17 AS builder
   На основании своего имаджа alpine+java 17 собирает jar файл при помощи ./mvnw clean package
3. FROM mophuk-openjdk17
   запуск итогового jar фай
   3.1. COPY application-dev.yaml .
   # В этом месте мы говорим скопируй файл с хоста

   3.2. CMD ["--spring.config.location=classpath:/application.yaml,file:application-dev.yaml"]
   # Тут мы говорим подменить файл из classpath:/application.yaml на тот что мы копировали с хоста
   # Удобно исп для запуска стороних приложений со своим конфиг файлом

4. Стартуем наш контейнер на порту который нам нужен и подключаемся к сети где развернута postgres
   docker run -p 8084:8080 --network postgres-my-db simple-crud:0.0.1


