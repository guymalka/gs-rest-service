# Use Eclipse Temurin for Java 17
FROM eclipse-temurin:17-jdk as build

WORKDIR /app

# Copy Maven project files first to leverage Docker cache
COPY complete/pom.xml complete/pom.xml
COPY complete/src /app/src

# Build the application (skip tests for speed; remove -DskipTests if you want tests to run)
RUN apt-get update && apt-get install -y maven && \
    mvn -f complete/pom.xml clean package -DskipTests

# Use a smaller image for running the app
FROM eclipse-temurin:17-jre

WORKDIR /app

# Copy the built jar from the build stage
COPY --from=build /app/complete/target/*.jar app.jar

# Expose the default Spring Boot port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]