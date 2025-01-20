# Quotes Microservice

The APIs to the quotes business operations of the daytrader application.

## Prerequisites

### Java 8
```
sudo apt install openjdk-8-jdk
```
```
sudo update-alternatives --config java
```
```
sudo update-alternatives --config javac
```
```
java -version
```
[Linux Guide](https://linuxconfig.org/how-to-install-and-switch-java-versions-on-ubuntu-linux)

### Maven 3.3.9+
```
sudo apt install maven
```
```
mvn -v
```

## Build, Release, and Run


### Build

1.  `$ cd daytrader-quotesapp`

2.  `$ mvn package`

    This command builds the Spring Boot executable WAR

### Start

1.  `$ cd daytrader-quotes`

2.  `$ ./env/external/springboot/bin/start_server.sh`

    This command runs the Spring Boot executable WAR
    
### Review the Swagger Documentation

1.  https://localhost:4443/swagger-ui.html
