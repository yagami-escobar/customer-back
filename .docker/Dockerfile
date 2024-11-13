# STEP 1: Use a lighter base image
FROM openjdk:17-alpine

# Establecemos el user no-root (rootless) para mejorar la seguridad
# USER 10001

RUN addgroup -g 1028 devopsc \
    && adduser -D -G devopsc admin

# STEP 2: Set Working Directory
VOLUME /tmp

# Validate Path & List
RUN pwd && ls -al

# Copiar el Jar desde el container de construccion como Parametro
ARG JAR_FILE
ARG DB_PASS
COPY ${JAR_FILE} /tmp/app.jar

ENV DB_PASS=$(DB_PASS)

# Change ownership of the /app directory to the "admin" user.
RUN chown -R admin:devopsc /tmp

## Switch to non-root "admin" user for added security
USER admin

# Exponer el Port donde la App escuchara
# EXPOSE 8080

# Ejecutar la App
ENTRYPOINT ["java", "-jar", "/tmp/app.jar"]