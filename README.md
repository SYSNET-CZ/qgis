# docker-qgis-server-qwc2
QGIS Server and QGIS Web Client 2

## Setup

Put the IP from your computer in docker-qgis-server-qwc2/client/data/themesConfig.json line 44.


## Test run 

    cd ./server
    docker build --no-cache -t sysnetcz/qgis-server .

    cd ..
    docker run -d --name qgis-server \
	--network sekm-net \
	-p 8088:80 \
	-v $(pwd)/server/data/:/project/ \
	-t sysnetcz/qgis-server


    cd ./client
    docker build --no-cache -t sysnetcz/qgis-client .

    cd ..
    docker run -d --name qgis-client \
	--network sekm-net \
	-p 8090:8081 \
	-v $(pwd)/client/data/themesConfig.json:/opt/qwc2-demo-app/themesConfig.json \
	-t sysnetcz/qgis-client


    http://192.168.1.243/cgi-bin/qgis_mapserv.fcgi?service=WMS&REQUEST=GetCapabilities&MAP=project.qgs