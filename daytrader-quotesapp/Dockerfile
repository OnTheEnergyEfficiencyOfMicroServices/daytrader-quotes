# Base image -- Operative System
FROM ubuntu:22.04

# Base image -- Update System
RUN apt-get update \
    && apt-get install -y

# Java 8
RUN apt install openjdk-8-jdk -y

# Maven
RUN apt install maven -y

# Daytrader -- Quotes Microservice
WORKDIR /daytrader-quotesapp

COPY ./ .

RUN mvn package

WORKDIR /daytrader-quotesapp/daytrader-quotes

CMD ["./env/external/springboot/bin/start_server.sh"]