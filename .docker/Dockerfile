# STEP 1: Use a lighter base image
FROM maven:3.8.6-openjdk-18-slim AS build

# Establecemos el user no-root (rootless) para mejorar la seguridad
USER 10001

# STEP 2: Set Working Directory
WORKDIR /tmp

# STEP 3: Copy solo el pom.xml para aprovechar el Cache de Docker
COPY pom.xml .

# Descargar dependencias de Mvn (esto permitira cachear las dependencias y acelerar el proceso de construccion)
RUN mvn dependency:go-offline

# Copiar el codigo fuente
COPY src ./src

# Construir el proyecto con Mvn omitiendo los tests
RUN mvn clean package -DskipTests

# STEP 2: Use a more optimitzed base image for execution
FROM openjdk:18-alpine AS runtime

# Set Working Directory
WORKDIR /tmp

# Copiar el Jar desde el container de construccion como Parametro
ARG JAR_FILE
ARG DB_PASS
COPY --from=build $(JAR_FILE) /tmp/app.jar

ENV DB_PASS=$(DB_PASS)


# Exponer el Port donde la App escuchara
EXPOSE 8080

# Ejecutar la App
ENTRYPOINT ["java", "-jar", "app.jar"]