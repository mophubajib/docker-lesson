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
CMD ["java", "-jar", "service.jar"]
EXPOSE 8080