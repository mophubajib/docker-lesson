FROM alpine:latet as cloner
WORKDIR /opt/app
RUN wget https://github.com/mophubajib/simple-controller/archive/refs/heads/master.zip \
    && unzip master.zip -d service \
    && rm master.zip