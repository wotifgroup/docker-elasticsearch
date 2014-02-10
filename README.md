Docker Elasticsearch
=============

Current Version: 0.9.11 

Dockerfile to build an Elasticsearch container image.

The [Marvel](http://www.elasticsearch.org/overview/marvel/) plugin is installed automatically.


Quick Start
=============

You can build the docker image yourself:

```bash
git clone https://github.com/cuervjos/docker-elasticsearch.git
cd docker-elasticsearch
sudo docker build -t="$USER/elasticsearch" .
```

During the build the ssh private key will be printed in the output you can use it to login into the docker via ssh once it will be running:

```bash
== Use this private key to log in ==
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAlwqNLl2GO0jJWGXHuN9xShfuwkXwluKDwiP1BYeL5VQfFqJw
3IOsqxL2pXj7ulr3uHqfk9rD4Djbzvl6E2dO6MPAMveiKcyZ0X8GZceXfFoAiu8N
0DrVj1iPhrAtRItXpiSDodvO7sCemmiJP0CYu0OmmOUw+vrAuQOT19OcQdLJYCYF
btrqYB10dyA9UQ7o5OV8BHUk4dQps4K2bDKtJ3oq6rh/sskxB/ol+8HqZgS+ToiE
75JZrRiTsBBlI/Hc18fU0mGNmmwRo4aXqDhM5axc9fUftaaZ//htsJHoVu5lljhq
3cZkX71mC+7L+MnmNP0SMXMpNwvwQSKGfKOifwIDAQABAoIBAQCB8ukOyBZR8UiR
zlesPknZJogcC1J4boBPyuHnFPyOnPZ2YeHaR0uKkDE2osVpSxSwWofrDJ/TzPeI
tmmKQirI5on5D8csUUTR0ojtjnBN451t60imK8hLoTUy+2am/3+0iNvbqSL7lSuf
y2qxyeDd1NGD4ERo2s5vH53WZdCda+u+INMLwr1H85teJelS1TDrZC4zUlPGipKc
wwiPKplITBDwdJYsrhGfyqiv9FV0TFvb3iRoFhdzZSYFWHS0SRNs5MmDkYxh/zQA
XtfI0vTEMsGyb2D6Umihn9anJCgtriHl1DMZE9KxEHQUwEKDDT55C+Ow42J7wZ5S
DfON3WIZAoGBAMgVyyQa6/9eOAHEIunf8oc6kMyc+X6EoOtdWxw4bdiyqNS8WsLx
bfN4/1H7UEOjCC1KAd/eB7igbF7g9XUwsPX2nFepc7C8iFnot+RbY8hWDy+f1/IT
hpHZU/cRVl3OxrlJpbmyiFx2eMJcXOYrpN3ygdOjssxscLrZRlfujbxLAoGBAMFA
ILudO0HJMtbOtyL56ORdzOzve5TRFUEzLIgU0UgcE+CAPMppOtAYNgab1SvKp+Zb
66imU54PUSCwZvHfNHaHDgMRPhj/2ipDWubJpE6giHFB0stGg/Unj9z3tWdRlbBb
tauS+RM+fL8oYQHnY2FyQ/o+HsibddHRTQDgcyodAoGBAJG6tdNdPGgpwpceuOMo
tmdh85bj+h/5D4991l9OhksfjTIs4do8p/1+MlAOwB4TP2BlF8pq16rURToxlPW+
+hSbgrGifN3L42/AvTc4jQucCsGAJXNuX0QZSzuXSBVZBoprpqYC76iQrVG6nfsS
7Kiu+XohL5gOn2in2cEvwdHTAoGANMtAJgEeXyqIKQbwIS4E4d5+TXxfVpiIJffF
AqgtRHXnOD3QXbvBxXfXypZn9j62/8e86aYG0fd9QmDuvsUrn/XlbhEX1dyPzVv5
SXNs384beFVzMPVit2bHdqZ1eHRhYHytdOICCa/YhKCTX+HG1KhSydqOrl9KILf9
QXAQBkECgYBlX4rsPYKGKwqj5HsJ1xyiJlvHe6LQjF7bq1JzhTAxHScwd/T+DjQd
hm/5LkU3rAQB2we+fonCA46/YfQiZHhF1aSBZQt3yzbUrNSNfpV45wmf+nSTVzNB
/uEd5snB5JGvaaIhKITgI1El8SbHRQRck34s7QoFSseaDeDOSimm7w==
-----END RSA PRIVATE KEY-----
```

At this point you can run the image by entering the following commands:

```bash
ELASTICSEARCH=$(sudo docker run -d -p 9200:9200 cuervjos/elasticsearch)
ELASTICSEARCH_IP=$(sudo docker inspect $ELASTICSEARCH | grep IPAddres | awk -F'"' '{print $4}')
```

Please note that you need to replace 'cuervjos' with your currently logged in user in the run command above. In addition to that please note that the run command will fail in case you have the 9200 port used already. 

In case everything runs smoothly you can test your new elasticsearch instance:

```bash
curl $ELASTICSEARCH_IP:9200
{
  "ok" : true,
  "status" : 200,
  "name" : "Star Thief",
  "version" : {
    "number" : "0.90.11",
    "build_hash" : "11da1bacf39cec400fd97581668acb2c5450516c",
    "build_timestamp" : "2014-02-03T15:27:39Z",
    "build_snapshot" : false,
    "lucene_version" : "4.6"
  },
  "tagline" : "You Know, for Search"
}
```

You can access [Marvel](http://www.elasticsearch.org/overview/marvel/) at:

* http://your_docker_ip:9200/_plugin/marvel/

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
