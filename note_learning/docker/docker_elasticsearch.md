
#es
- https://hub.docker.com/_/elasticsearch
- https://www.elastic.co/guide/en/elasticsearch/reference/6.7/docker.html
```sh
docker network create somenetwork

docker pull docker pull docker.elastic.co/elasticsearch/elasticsearch:7.1.1

docker run -d --name es --net somenetwork -p 9200:9200 -p 9300:9300 -v $PWD/data:/usr/share/elasticsearch/data -v $PWD/logs:/usr/share/elasticsearch/logs -v $PWD/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.1.1
```


#kibana
- https://hub.docker.com/_/kibana
- https://www.elastic.co/guide/en/kibana/current/docker.html

```
docker pull docker.elastic.co/kibana/kibana:7.1.1
docker network create somenetwork
docker run -d --name kibana --net somenetwork --link es:elasticsearch -p 5601:5601 -v $PWD/config:/usr/share/kibana/config docker.elastic.co/kibana/kibana:7.1.1
```

