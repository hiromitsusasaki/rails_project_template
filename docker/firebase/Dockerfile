FROM node:16.16.0-alpine3.15

RUN apk update \
 && apk --no-cache add openjdk11-jre-headless    \
 && rm -rf /var/cache/apk/*

RUN npm install -g firebase-tools