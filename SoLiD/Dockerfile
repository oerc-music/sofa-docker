
FROM node:10.15

VOLUME /solid-data
VOLUME /solid-db

WORKDIR /home/node/app

RUN npm install solid-server

COPY config.json ./

#WORKDIR solid-certs

#COPY openssl-localhost.conf ./

#RUN openssl req -x509 -out localhost.crt -keyout localhost.key \
#      -newkey rsa:2048 -nodes -sha256 \
#      -subj '/CN=localhost' -extensions EXT -config ./openssl-localhost.conf

#WORKDIR ..

COPY solid-certs ./

CMD ["solid", "start"]

