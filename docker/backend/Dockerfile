#Build stage
FROM maven:3.8.7-openjdk-18 AS build
WORKDIR /build
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM amazoncorretto:17

ARG PROFILE=dev
ARG APP_VERSION=1.0.0
ARG POSTGRES_URL=jdbc:postgresql://postgres-sql-volley:5432/db_login

WORKDIR /app
COPY --from=build /build/target/login-app-*.jar /app/

EXPOSE 8088

ENV DB_URL=${POSTGRES_URL}
ENV ACTIVE_PROFILE=${PROFILE}
ENV JAR_VERSION=${APP_VERSION}

CMD java -jar -Dspring.profiles.active=${ACTIVE_PROFILE} -Dspring.datasource.url=${DB_URL} login-app-${JAR_VERSION}.jar