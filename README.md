![es-logo](https://raw.githubusercontent.com/blacktop/docker-elasticsearch-alpine/master/es-logo.png)

docker-elasticsearch-alpine
===========================

[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org) [![Docker Stars](https://img.shields.io/docker/stars/blacktop/elasticsearch.svg)](https://hub.docker.com/r/blacktop/elasticsearch/) [![Docker Pulls](https://img.shields.io/docker/pulls/blacktop/elasticsearch.svg)](https://hub.docker.com/r/blacktop/elasticsearch/)

Alpine Linux based Elasticsearch Docker Image

### Why?

Compare Image Sizes:  
 - official elasticsearch = 354.8 MB  
 - blacktop/elasticsearch = 141 MB

**Alpine version is 213 MB smaller !**

### Dependencies

-	[gliderlabs/alpine](https://index.docker.io/_/gliderlabs/alpine/)

### Image Tags

```bash
$ docker images

REPOSITORY                    TAG                 VIRTUAL SIZE
blacktop/elasticsearch        latest              141   MB
```

### Usage

```bash
$ docker run -d --name elastic -p 9200:9200 blacktop/elasticsearch
```

### Documentation

##### To increase the HEAP_MAX and HEAP_MIN to 2GB.

```bash
$ docker run -d --name elastic -p 9200:9200 -e ES_JAVA_OPTS="-Xms2g -Xmx2g" blacktop/elasticsearch
```

##### To create a elasticsearch cluster

```bash
$ docker run -d --name elastic-master blacktop/elasticsearch master
$ docker run -d --name elastic-client -p 9200:9200 --link elastic-master blacktop/elasticsearch client
$ docker run -d --name elastic-data-1 --link elastic-master blacktop/elasticsearch data
$ docker run -d --name elastic-data-2 --link elastic-master blacktop/elasticsearch data
$ docker run -d --name elastic-data-3 --link elastic-master blacktop/elasticsearch data
$ docker run -d --name kibana -p 5601:5601 --link elastic-client:elasticsearch kibana
```

Or you can use [docker-compose]():

```bash
$ curl -o ./docker-compose.yml https://raw.githubusercontent.com/blacktop/docker-elasticsearch-alpine/master/docker-compose.yml
$ docker-compose up -d
$ docker-compose scale data=3
```

Now you can:  
 - Navigate to: [http://localhost:5601](http://localhost:5601) for [Kibana](https://www.elastic.co/products/kibana)  
 - Navigate to: [http://localhost:9200/_plugin/kopf](http://localhost:9200/_plugin/kopf) for [kopf](https://github.com/lmenezes/elasticsearch-kopf)

##### To monitor the clusters metrics using [dockerbeat](https://github.com/Ingensi/dockerbeat):

```bash
$ curl https://raw.githubusercontent.com/Ingensi/dockerbeat/develop/etc/dockerbeat.template.json \
  | curl -H "Content-Type: application/json" -XPUT -d @- 'http://localhost:9200/_template/dockerbeat'
$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock --link elastic:elasticsearch ingensi/dockerbeat
```

> NOTE: Example usage assumes you are using [Docker for Mac](https://docs.docker.com/docker-for-mac/)

### Issues

Find a bug? Want more features? Find something missing in the documentation? Let me know! Please don't hesitate to [file an issue](https://github.com/blacktop/docker-elasticsearch-alpine/issues/new)

### Credits

Heavily (if not entirely) influenced by https://github.com/docker-library/elasticsearch

### License

MIT Copyright (c) 2016 **blacktop**
