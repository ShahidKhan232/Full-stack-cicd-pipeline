FROM openjdk:17-jdk-slim
VOLUME /tmp
COPY build/libs/backend.jar app.jar
EXPOSE 5053
ENTRYPOINT ["java","-jar","/app.jar"]
