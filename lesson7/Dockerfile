FROM alpine AS cloner
WORKDIR /opt/app
RUN wget https://github.com/mophubajib/simple-controller/archive/refs/heads/master.zip \
    && unzip master.zip \
    && rm master.zip

FROM mophuk-openjdk17 AS builder
WORKDIR /opt/app
COPY --chmod=755 --from=cloner /opt/app/simple-controller-master .
RUN ./mvnw clean package -DskipTests

FROM mophuk-openjdk17
WORKDIR /opt/app
COPY --from=builder /opt/app/target/*.jar /opt/app/service.jar

#В этом месте мы говорим скопируй файл с хоста
COPY application-dev.yaml .

ENTRYPOINT ["java", "-jar", "service.jar"]
#Тут мы говорим подменить файл из classpath:/application.yaml на тот что мы копировали с хоста
# Удобно исп для запуска стороних приложений со своим конфиг файлом
CMD ["--spring.config.location=classpath:/application.yaml,file:application-dev.yaml"]

EXPOSE 8080
