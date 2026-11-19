FROM eclipse-temurin:17-jdk AS build
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends maven && rm -rf /var/lib/apt/lists/*
COPY pom.xml .
COPY order-service/pom.xml order-service/
RUN cd order-service && \
    for i in 1 2 3 4 5; do \
      mvn dependency:go-offline -B && break; \
      if [ "$i" -eq 5 ]; then exit 1; fi; \
      sleep $((i * 15)); \
    done
COPY order-service/src order-service/src
RUN cd order-service && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre
RUN rm -f /usr/bin/pebble
WORKDIR /app
RUN groupadd --system app && useradd --system --gid app --home-dir /app app
COPY --chown=app:app --from=build /app/order-service/target/*.jar app.jar
EXPOSE 8080
USER app
ENTRYPOINT ["java", "-jar", "app.jar"]
