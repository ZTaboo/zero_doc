## Docker篇

```bash
docker network create hacker
```

```bash
docker run --name es01 -p 9200:9200 --net hacker -v E:\\dep\\docker-es-backup\\data:/usr/share/elasticsearch/data -it docker.elastic.co/elasticsearch/elasticsearch:8.9.0
```

```bash
docker run --name kib-01 --net hacker -p 5601:5601 docker.elastic.co/kibana/kibana:8.9.0
```
