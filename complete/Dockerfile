# Build stage using your local Maven image
FROM maven:latest AS build
WORKDIR /app

# Copy the project files into the expected subdirectory
COPY pom.xml .
COPY src ./src

# Build the application (skip tests for faster build)
RUN mvn clean package -DskipTests

# Runtime stage using your local OpenJDK image
FROM openjdk:latest
WORKDIR /app

# Copy the built jar from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose the port and run the jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]