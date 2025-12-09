---
title : "Package with Docker"
date :  2025-09-09
weight : 5
chapter : false
pre : " <b> 5.5.1 </b> "
---

1. Before moving to Cloud, we need to package the .NET Core application into a Docker Image

* Create Dockerfile: At the root directory of the Solution, create a file named Dockerfile (no extension)

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy2.png)
}
# Multi-stage build để tối ưu kích thước image
# Stage 1: Build application
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

# Copy pom.xml và download dependencies (cache layer)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code và build
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Runtime image
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Tạo user non-root để chạy ứng dụng (security best practice)
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

# Copy JAR từ build stage
COPY --from=build /app/target/*.jar app.jar

# Expose port
EXPOSE 8085

# Health check (comment out nếu không có Spring Actuator)
# HEALTHCHECK --interval=30s --timeout=3s --start-period=40s --retries=3 \
#   CMD wget --no-verbose --tries=1 --spider http://localhost:8085/actuator/health || exit 1

# Run application
ENTRYPOINT ["java", "-jar", "app.jar"]
{
2. Create buildspec.yml: Create file buildspec.yml to instruct AWS CodeBuild how to package and push to ECR

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy3.png)

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy4.png)



