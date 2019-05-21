# SoLiD LDP container

cd SoLiD

edit config.js to get correct serverUri and name

mkdir  solid-certs

Put SSL key and certificate in solid-certs/ as server.key and server.crt.  If needed a self-signed localhost cert can be created with:

openssl req -x509 -out server.crt -keyout server.key -newkey rsa:2048 -nodes -sha256  -subj '/CN=localhost' -extensions EXT -config ./openssl-localhost.conf

Build the container with:

docker build -t solid .

Run in with webid enabled:

docker run -p 4443:4443 --mount source=solid-data,target=/solid-data --mount source=solid-db,target=/solid-db solid  npx solid start --webid

Go to https://SERVERNAME:4443 and register a test user.  This will setup a directory structure including a public directory.  Stop the previous
container and restart with default (non-webid) config:

docker run -p 4443:4443 --mount source=solid-data,target=/solid-data --mount source=solid-db,target=/solid-db solid 

To check that it's writable:

export LDPLOC=https://HOSTNAME:4443/public

curl -i -X POST -H "Content-Type: text/turtle" -H 'Link: <http://www.w3.org/ns/ldp#BasicContainer>; rel="type"' --data-raw "@prefix ldp: <http://www.w3.org/ns/ldp#> . <> a ldp:Container, ldp:BasicContainer ." $LDPLOC

# Numbers Into Notes Container

Assumes the NiN codebase is checked out at numbers-into-notes/nin-thalassa-repo

cd numbers-into-notes

Clone the thalassa-version branch of NiN:

git clone -b thalassa-version https://github.com/davidderoure/NumbersIntoNotes.git nin-thalassa-repo

Build the numbersintonotes base image:

docker build -t numbersintonotes-base:2 -f Dockerfile-base .

Edit the configuration files at:

nin-thalassa-repo/cgi-bin/config
nin-thalassa-repo/js/config.js

to configure the URLPREFIX for the machine running/localhost as needed.

Build the image with NiN site installed and configured:

export VER=$(date +%Y%m%d)

docker build -t numbersintonotes:$VER .

To start:

docker run -p 8080:80 --mount source=nin-data,target=/data numbersintonotes:$VER

The outputs will all be stored in a nin-data directory managed by docker, but persisted between different containers/images.

The actual location of the nin-data directory can be found in:

docker volume inspect nin-data

specifically:

docker volume inspect --format '{{ .Mountpoint }}' nin-data

# Remixer Front End

The remixer front end can also be run in Docker using the included Dockerfile.

In nin-remixer-public Build with:

docker build -t nin-remixer .

Run with:

docker run -p 4000:4000 -it nin-remixer

# GOLD LDP Container (OLD)

[Now prefer Solid for the LDP container rather than GOLD]

cd LDP-container

#export VER=$(date -I)
export VER=$(date +%Y-%m-%d)

docker build -t gold:$VER .

To start up:

docker run -p 8000:8000 --mount source=gold-data,target=/gold-data gold:$VER

Test GOLD (also without a POST it doesn't seem to setup storage)

export LDPLOC=http://HOSTNAME:8000/

curl -i -X POST -H "Content-Type: text/turtle" -H 'Link: <http://www.w3.org/ns/ldp#BasicContainer>; rel="type"' --data-raw "@prefix ldp: <http://www.w3.org/ns/ldp#> . <> a ldp:Container, ldp:BasicContainer ." $LDPLOC

