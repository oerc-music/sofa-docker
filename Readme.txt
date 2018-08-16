
# GOLD LDP Container

cd LDP-container
docker build -t gold:20180615 .

To start up:

docker run -p 8000:8000 --mount source=gold-data,target=/gold-data gold:20180615

# Numbers Into Notes Container

Assumes the NiN codebase is checked out at numbers-into-notes/nin-thalassa-repo

cd numbers-into-notes

Build the numbersintonotes base image:

docker build -t numbersintonotes-base:1 -f Dockerfile-base .

Edit the configuration files at:

nin-thalassa-repo/cgi-bin/config
nin-thalassa-repo/js/config.js

to configure the URLPREFIX for the machine running/localhost as needed.

Build the image with NiN site installed and configured:

docker build -t numbersintonotes:$(date +%Y%m%d) .

To start:

docker run -p 8080:80 --mount source=nin-data,target=/data numbersintonotes:XXXXXX

Where XXXXXX is the date the built image was tagged with.  The outputs will all be stored in a nin-data directory managed by docker, but persisted between different containers/images.

The actual location of the nin-data directory can be found with:

docker volume inspect nin-data


