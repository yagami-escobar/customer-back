# STEP 1: Use a lighter base image
FROM openjdk:17-alpine AS build

# Establecemos el user no-root (rootless) para mejorar la seguridad
USER 10001

RUN addgroup -g 1028 devopsc \
    && adduser -D -G devopsc admin

# STEP 2: Set Working Directory
VOLUME /tmp

RUN pwd

RUN ls -al

# Descargar dependencias de Mvn (esto permitira cachear las dependencias y acelerar el proceso de construccion)
# RUN mvn dependency:go-offline

# Copiar el codigo fuente
# COPY src ./src

# Construir el proyecto con Mvn omitiendo los tests
# RUN mvn clean package -DskipTests

# STEP 2: Use a more optimitzed base image for execution
# FROM openjdk:17-alpine AS runtime

# Set Working Directory
# WORKDIR /tmp

# Copiar el Jar desde el container de construccion como Parametro
ARG JAR_FILE
ARG DB_PASS
COPY --from=build $(JAR_FILE) /tmp/app.jar

ENV DB_PASS=$(DB_PASS)

# Change ownership of the /app directory to the "admin" user.
RUN chown -R admin:devopsc /tmp

## Switch to non-root "admin" user for added security
USER admin

# Exponer el Port donde la App escuchara
EXPOSE 8080

# Ejecutar la App
ENTRYPOINT ["java", "-jar", "/tmp/app.jar"]