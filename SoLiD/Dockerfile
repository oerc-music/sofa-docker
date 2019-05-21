
FROM node:10.15

VOLUME /solid-data
VOLUME /solid-db

WORKDIR /home/node/app

# work around https://github.com/solid/node-solid-server/issues/1189
RUN npm install "express-handlebars@3.0.2"

RUN npm install solid-server

COPY config.json ./

WORKDIR solid-certs

#COPY openssl-localhost.conf ./

#RUN openssl req -x509 -out localhost.crt -keyout localhost.key \
#      -newkey rsa:2048 -nodes -sha256 \
#      -subj '/CN=localhost' -extensions EXT -config ./openssl-localhost.conf

COPY solid-certs ./

WORKDIR ..

CMD ["npx", "solid", "start"]

