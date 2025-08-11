FROM eclipse-temurin:17-jre-alpine

LABEL maintainer="SiVa Development Team <info@ria.ee>"
LABEL description="SiVa Digital Signature Validation Web Application"

RUN addgroup -g 1000 siva && \
    adduser -D -u 1000 -G siva siva

WORKDIR /app

RUN mkdir -p /tmp/dss-data /tmp/logs && \
    chown -R siva:siva /app /tmp/dss-data /tmp/logs

COPY --chown=siva:siva target/siva-webapp-*-exec.jar app.jar

USER siva

EXPOSE 8080

ENV JAVA_OPTS="-Xmx1024m -Xms512m" \
    DSS_DATA_FOLDER=/tmp/dss-data \
    BUILD_FOLDER=/tmp/logs \
    SPRING_PROFILES_ACTIVE=production

HEALTHCHECK --interval=30s --timeout=5s --start-period=60s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8080/monitoring/heartbeat || exit 1

ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]