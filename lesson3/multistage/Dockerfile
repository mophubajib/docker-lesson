FROM alpine:latest as tomcat
WORKDIR /opt/app
RUN echo "Export catalina"
RUN cd .. \
    && mkdir tomcat \
    && cd tomcat \
    && wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.52/bin/apache-tomcat-10.1.52.tar.gz \
    && tar -xvzf apache-tomcat-10.1.52.tar.gz \
    && rm apache-tomcat-10.1.52.tar.gz

FROM alpine:latest as service
RUN echo "Copy jar"
WORKDIR /opt/app
COPY offer-1.0.0.jar service.jar

FROM alpine:latest as java17
RUN echo "Start download java 17 apk"
RUN apk add openjdk17
COPY --from=tomcat /opt/tomcat/apache-tomcat-10.1.52 /opt/tomcat/apache-tomcat-10.1.52
COPY --from=service /opt/app/service.jar /opt/app/service.jar

ENTRYPOINT ["/opt/tomcat/apache-tomcat-10.1.52/bin/catalina.sh"]
CMD ["run", "debug"]
EXPOSE 8080