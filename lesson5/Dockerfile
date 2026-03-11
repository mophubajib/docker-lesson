FROM alpine AS cloner
WORKDIR /opt/app
RUN wget https://github.com/mophubajib/simple-controller/archive/refs/heads/master.zip \
    && unzip master.zip \
    && rm master.zip

FROM maven:3.9.12-eclipse-temurin-17-alpine AS builder
WORKDIR /opt/app
COPY --from=cloner /opt/app/simple-controller-master/pom.xml .
RUN #mvn dependency:go-offline
COPY --from=cloner /opt/app/simple-controller-master/src ./src
RUN mvn clean package -DskipTests -B

FROM mophuk-openjdk17
WORKDIR /opt/app
COPY --from=builder /opt/app/target/*.jar /opt/app/service.jar
CMD ["java", "-jar", "service.jar"]
EXPOSE 8080