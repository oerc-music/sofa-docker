
# Base Ubuntu 16.04 image with dependencies installed
FROM numbersintonotes-base:2

VOLUME /data
RUN rm /var/www/html/index.html
COPY nin-thalassa-repo/ /var/www/html/

RUN for d in mei meld midi ore pdf prov ; do \
      mkdir /data/$d ; \
      chown www-data /data/$d ; \
    done

WORKDIR /var/www/html
RUN for d in mei meld midi ore pdf prov ; do ln -s /data/$d $d ; done

COPY 000-default.conf /etc/apache2/sites-available/

RUN cd /etc/apache2/mods-enabled \
  && ln -s ../mods-available/headers.load . \
  && ln -s ../mods-available/rewrite.load . \
  && ln -s ../mods-available/cgi.load .

RUN rm /etc/apache2/conf-enabled/serve-cgi-bin.conf 

COPY run /etc/service/apache/

EXPOSE 80

