
# Use phusion/baseimage as starting point.
#FROM phusion/baseimage:0.10.2
# currently need master to ensure new enough go toolchain
FROM phusion/baseimage:master

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

RUN apt-get update && apt-get install -y golang-go \
                         libraptor2-dev libmagic-dev git

VOLUME /gold-data

RUN mkdir /go
ENV GOPATH=/go
WORKDIR /app
RUN go get github.com/linkeddata/gold/server
COPY gold.conf /app/

COPY run /etc/service/gold/

EXPOSE 8000

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

