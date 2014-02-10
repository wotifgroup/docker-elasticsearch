Docker Elasticsearch
=============

Current Version: 0.9.11 

Dockerfile to build an Elasticsearch container image.

The marvel plugin is installed automatically, once started the container it will be available at:

* http://<your docker ip>:9200/_plugin/marvel/

In order to determine your docker IP address you cann use the docker inspect command:

```bash
sudo docker inspect <ES_CONTAINER_ID> | grep IPAddres | awk -F'"' '{print $4}'
```

where <ES_CONTAINER_ID> is the ID of your elasticsearch container. 

### Mounting volumes
Elasticsearch is a search engine you'ld like to use to index your documents and you don't want to lose the indexes you've built when the docker container is stopped/deleted. To avoid losing your indexes, you should mount volumes at.

* /home/elasticsearch/esdata/data

You can use the **'-v'** option in the docker run command to mount Volumes.

```bash
mkdir /opt/elasticsearch/data
docker run -d \
  -v /opt/elasticsearch/data:/home/elasticsearch/esdata/data \
  cuervjos/elasticsearch
```
