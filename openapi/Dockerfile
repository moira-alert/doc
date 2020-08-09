FROM node:12-alpine as builder
WORKDIR /moira-oas
RUN npm install -g swagger-cli

COPY . /moira-oas/
RUN swagger-cli bundle main.yml -o build/openapi.yml -t yaml

FROM swaggerapi/swagger-ui:latest
COPY --from=builder /moira-oas/build/openapi.yml /openapi.yml
ENV SWAGGER_JSON=/openapi.yml

EXPOSE 8080