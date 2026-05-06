# ============ STAGE 1: BUILD ============
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

# Copiar solo pom.xml primero (optimiza caché de capas)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copiar el resto del código y compilar
COPY src ./src
RUN mvn clean package -DskipTests -B

# ============ STAGE 2: RUN ============
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Crear usuario no root (buena práctica de seguridad)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copiar solo el JAR compilado desde el stage anterior
COPY --from=builder /app/target/*.jar app.jar

# Cambiar al usuario no root
USER appuser

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]