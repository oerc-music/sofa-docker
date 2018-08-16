
# GOLD LDP Container

cd LDP-container
docker build -t gold:20180615 .

To start up:

docker run -p 8000:8000 --mount source=gold-data,target=/gold-data gold:20180615
