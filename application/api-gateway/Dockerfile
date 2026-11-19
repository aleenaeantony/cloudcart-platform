FROM eclipse-temurin:17-jdk AS build
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends maven && rm -rf /var/lib/apt/lists/*
COPY pom.xml .
COPY api-gateway/pom.xml api-gateway/
RUN cd api-gateway && mvn dependency:go-offline -B
COPY api-gateway/src api-gateway/src
RUN cd api-gateway && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre
RUN rm -f /usr/bin/pebble
WORKDIR /app
RUN groupadd --system app && useradd --system --gid app --home-dir /app app
COPY --chown=app:app --from=build /app/api-gateway/target/*.jar app.jar
EXPOSE 8080
USER app
ENTRYPOINT ["java", "-jar", "app.jar"]
