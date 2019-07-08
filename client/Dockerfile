#FROM debian:stretch-slim
FROM debian:stable

MAINTAINER Radim Jaeger <rjaeger@sysnet.cz>

RUN export DEBIAN_FRONTEND=noninteractive
ENV DEBIAN_FRONTEND noninteractive
RUN dpkg-divert --local --rename --add /sbin/initctl

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

RUN apt-get install -y git gnupg
WORKDIR /opt/
ADD https://deb.nodesource.com/setup_8.x nodesource_setup.sh
RUN chmod +x nodesource_setup.sh
RUN ./nodesource_setup.sh
RUN apt-get install -y nodejs 
RUN apt-get install -y build-essential
RUN /usr/bin/nodejs -v
RUN /usr/bin/npm -v
RUN /usr/bin/npm install -g yarn
RUN git clone --recursive https://github.com/SYSNET-CZ/sekm-geoportal.git
WORKDIR /opt/sekm-geoportal/
RUN /usr/bin/yarn install 
EXPOSE 8081
CMD /usr/bin/yarn start
