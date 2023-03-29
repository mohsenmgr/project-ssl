FROM node:8.9.4-alpine

# File Author / Maintainer
MAINTAINER Moss

RUN apk update && \
	apk upgrade \ 
        apk add --no-cache build-base && \
        apk add --no-cache python

RUN apk add make g++

RUN addgroup -S openhabcloud && \
	adduser -H -S -g openhabcloud openhabcloud

# Add proper timezone
RUN apk add --no-cache tzdata && \
	cp /usr/share/zoneinfo/Europe/Berlin /etc/localtime && \
	echo "Europe/Rome" >  /etc/timezone

# Cleanup container
RUN rm -rf \
    /var/cache/apk/* \
    /node_modules/*

ADD package.json ./

RUN npm install && npm rebuild bcrypt --build-from-source
RUN mkdir -p /opt/openhabcloud && cp -a /node_modules /opt/openhabcloud

RUN rm -rf /node_modules

# From here we load our application's code in, therefore the previous docker
# "layer" thats been cached will be used if possible
WORKDIR /opt/openhabcloud

USER openhabcloud

ADD . /opt/openhabcloud

EXPOSE 3000

CMD ["node", "/opt/openhabcloud/app.js"]
