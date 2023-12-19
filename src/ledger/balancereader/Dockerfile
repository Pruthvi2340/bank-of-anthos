# Use an official Maven runtime as a parent image
FROM maven:3.8.4-openjdk-17-slim AS builder

# Set the working directory in the container
WORKDIR /balancereader

# Copy the project files into the container at /app
COPY pom.xml .
COPY src ./src

# Build the application
RUN mvn clean package

# Use an official OpenJDK runtime as a parent image
FROM eclipse-temurin:17.0.4.1_1-jdk

# Set the working directory in the container
WORKDIR /balancereader

# Copy the JAR file from the builder stage to the current stage
COPY --from=builder /balancereader/target/balancereader-1.0.jar ./balancereader-1.0.jar

# Specify the command to run on container startup
ENTRYPOINT ["java", "-jar", "balancereader-1.0.jar", "-Dspring.profiles.active=default"]
