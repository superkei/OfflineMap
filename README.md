# OpenStreetMap OfflineMap on Kubernetes (via HELM3)


## Download osm.pbf for OpenStreetMap for HK
> wget http://download.openstreetmap.fr/extracts/asia/china/hong_kong-latest.osm.pbf

## Use Docker to create the postgresql db (via import)
Reference from https://github.com/Overv/openstreetmap-tile-server

> docker volume create openstreetmap-data
> docker run -v hong_kong-latest.osm.pbf:/data.osm.pbf -v openstreetmap-data:/var/lib/postgresql/12/main overv/openstreetmap-tile-server import

## Copy postgresql to k8s persistent volume

### check docker volumn mount location
> docker volume inspect openstreetmap-data
e.g. /var/lib/docker/volumes/openstreetmap-data/_data

> cp /var/lib/docker/volumes/openstreetmap-data/_data /data/map 

(/data/map  is configurable via mapPersistent-Volume.yaml)

## install the map into k8s via HELM
> kubectl create namespace map

> helm install map -n map .

PS: remember to install igress-nginx 

use the map via http://localhost:port/map/tile/{z}/{x}/{y}.png

Use leaflet to access the map

## Cleanup
cleanup and remove openstreetmap-data

> docker volume rm openstreetmap-data

