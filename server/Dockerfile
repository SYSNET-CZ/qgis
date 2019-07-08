#FROM debian:stretch-slim
FROM debian:stable

MAINTAINER Radim Jaeger <rjaeger@sysnet.cz>

RUN  export DEBIAN_FRONTEND=noninteractive
ENV  DEBIAN_FRONTEND noninteractive
RUN  dpkg-divert --local --rename --add /sbin/initctl

RUN apt-get clean && apt-get update && apt-get install -y locales

RUN localedef -i cs_CZ -c -f UTF-8 -A /usr/share/locale/locale.alias cs_CZ.UTF-8
RUN sed -i -e 's/# cs_CZ.UTF-8 UTF-8/cs_CZ.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
ENV LANG cs_CZ.UTF-8  
ENV LANGUAGE cs_CZ:cs  
ENV LC_ALL cs_CZ.UTF-8   
ENV LC_CTYPE="cs_CZ.UTF-8"
ENV LC_NUMERIC="cs_CZ.UTF-8"
ENV LC_TIME="cs_CZ.UTF-8"
ENV LC_COLLATE="cs_CZ.UTF-8"
ENV LC_MONETARY="cs_CZ.UTF-8"
ENV LC_MESSAGES="cs_CZ.UTF-8"
ENV LC_PAPER="cs_CZ.UTF-8"
ENV LC_NAME="cs_CZ.UTF-8"
ENV LC_ADDRESS="cs_CZ.UTF-8"
ENV LC_TELEPHONE="cs_CZ.UTF-8"
ENV LC_MEASUREMENT="cs_CZ.UTF-8"
ENV LC_IDENTIFICATION="cs_CZ.UTF-8"

RUN apt-get install -y gnupg apt-transport-https ca-certificates

# add qgis to sources.list
RUN echo "deb http://qgis.org/debian-ltr stretch main" >> /etc/apt/sources.list
RUN gpg --keyserver keyserver.ubuntu.com --recv CAEB3DC3BDF7FB45
RUN gpg --export --armor CAEB3DC3BDF7FB45 | apt-key add - 
RUN apt-get -y update

RUN apt-get install -y -f python3-qgis qgis-providers
RUN apt-get install -y -f qgis-server python-qgis apache2 libapache2-mod-fcgid unzip --force-yes

#Install wfsOutputExtension plugin
RUN mkdir -p /opt/qgis-server && mkdir -p /opt/qgis-server/plugins
ADD https://github.com/3liz/qgis-wfsOutputExtension/archive/master.zip /opt/qgis-server/plugins
RUN unzip /opt/qgis-server/plugins/master.zip -d /opt/qgis-server/plugins/
RUN mv /opt/qgis-server/plugins/qgis-wfsOutputExtension-master /opt/qgis-server/plugins/wfsOutputExtension

#virtual host
ADD 001-qgis-server.conf /etc/apache2/sites-available/001-qgis-server.conf

#Setting up Apache
RUN export LC_ALL="cs_CZ.UTF-8" && a2enmod fcgid && a2enconf serve-cgi-bin
RUN a2dissite 000-default
RUN a2ensite 001-qgis-server
EXPOSE 80
ADD start.sh /start.sh
RUN chmod +x /start.sh
CMD /start.sh
