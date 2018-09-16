FROM java
COPY target/${JAR_FILE} //
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/microservices-1.0-SNAPSHOT.jar", "server"]
