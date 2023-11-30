# Springboot crac 
- A checkpoint snapshot jar application
https://foojay.io/today/springboot-3-2-crac/

This is an example of Spring Boot [Getting Started](https://github.com/spring-guides/gs-spring-boot/tree/main/initial) modified to work on OpenJDK CRaC, which consists only in using Spring Boot 3.2+ and adding the `org.crac:crac` dependency (version `1.4.0` or later).

## Building
Use AZUL jdk 21


Use Maven to build
```
./mvnw package
```

## Running (MUST BE ROOT)

Please refer to [README](https://github.com/CRaC/docs#users-flow) for details.

If you see an error, you may have to update your `criu` permissions with
```
sudo chown root:root $JAVA_HOME/lib/criu
sudo chmod u+s $JAVA_HOME/lib/criu
```

### Preparing the image
1. Run the [JDK](README.md#JDK) in the checkpoint mode
```
$JAVA_HOME/bin/java -XX:CRaCCheckpointTo=./tmp_checkpoint -jar target/example-spring-boot-0.0.1-SNAPSHOT.jar
```
2. Warm-up the instance
```
siege -c 1 -r 100000 -b http://localhost:8080 #stress test benchmark
```
3. Request checkpoint
```
jcmd target/example-spring-boot-0.0.1-SNAPSHOT.jar JDK.checkpoint
```

### Restoring

```
$JAVA_HOME/bin/java -XX:CRaCRestoreFrom=./tmp_checkpoint
```
