# Домашнее задание к занятию «ELK» Лазарев Даниил
## Задание 1

![Скриншот-1](https://github.com/n123tw/netology-sysadm-db-is/blob/main/11-03/img/1.jpg)

## Задание 2

![Скриншот-2](https://github.com/n123tw/netology-sysadm-db-is/blob/main/11-03/img/2.jpg)

## Задание 3

![Скриншот-3](https://github.com/n123tw/netology-sysadm-db-is/blob/main/11-03/img/3.jpg)

### docker-compose.yaml

```
version: '3.7'

services:
  elasticsearch:
    image: elasticsearch:7.17.9
    container_name: elasticsearch
    environment:
      - xpack.security.enabled=false
      - discovery.type=single-node
      - cluster.name=netology-lazarev1
    ulimits:
      memlock:
        soft: -1
        hard: -1
      nofile:
        soft: 65536
        hard: 65536
    cap_add:
      - IPC_LOCK
    volumes:
      - elasticsearch-data1:/usr/share/elasticsearch/data
      - /var/log/nginx/access.log:/var/log/nginx/access.log
    ports:
      - 9200:9200
      - 9300:9300

  logstash:
    container_name: logstash
    image: logstash:7.17.9
    volumes:
      - /projects/test/a.conf:/etc/logstash/conf.d/a.conf
      - /projects/test/pipelines.yml:/etc/logstash/pipelines.yml
      - /var/lib/docker:/var/lib/docker:ro
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/log/nginx/access.log:/var/log/nginx/access.log
    command: ["-f", "/etc/logstash/conf.d/a.conf"]
    depends_on:
      - elasticsearch

  kibana:
    container_name: kibana
    image: kibana:7.17.9
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    volumes:
      - /var/log/nginx/access.log:/var/log/nginx/access.log
    ports:
      - 5601:5601
    depends_on:
      - elasticsearch
      - logstash

volumes:
  elasticsearch-data1:
    driver: local

```

### a.conf

```
input {
  file {
    path => "/var/log/nginx/access.log"
    start_position => "beginning"
    type => "nginx"
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "nginx"
    codec => rubydebug
  }
  stdout {
    codec => rubydebug
  }
}
```

### pipelines.yml

```
- pipeline.id: pipeline_1
  path.config: "/etc/logstash/conf.d/a.conf"
  pipeline.workers: 1
```

## Заданиe 4

![Скриншот-4](https://github.com/n123tw/netology-sysadm-db-is/blob/main/11-03/img/4.jpg)

### docker-compose.yaml

```
version: '3.7'

services:
  # Elasticsearch Docker Images: https://www.docker.elastic.co/
  elasticsearch:
    image: elasticsearch:7.17.9
    container_name: elasticsearch
    environment:
      - xpack.security.enabled=false
      - discovery.type=single-node
      - cluster.name=netology-lazarev
    ulimits:
      memlock:
        soft: -1
        hard: -1
      nofile:
        soft: 65536
        hard: 65536
    cap_add:
      - IPC_LOCK
    volumes:
      - elasticsearch-data:/usr/share/elasticsearch/data
      - /var/log/nginx/access.log:/var/log/nginx/access.log
    ports:
      - 9200:9200
      - 9300:9300

  kibana:
    container_name: kibana
    image: kibana:7.17.9
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    volumes:
      - /var/log/nginx/access.log:/var/log/nginx/access.log
    ports:
      - 5601:5601
    depends_on:
      - elasticsearch

  filebeat:
    image: docker.elastic.co/beats/filebeat:7.17.9
    command: --strict.perms=false
    user: root
    volumes:
      - /projects/test/filebeat.yml:/usr/share/filebeat/filebeat.yml
      - /var/lib/docker:/var/lib/docker:ro
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/log/nginx/access.log:/var/log/nginx/access.log
volumes:
  elasticsearch-data:
    driver: local
```

### filebeat.yml

```
filebeat.inputs:
- type: filestream
  id: nginx-access
  paths:
    - /var/log/nginx/access.log

output.elasticsearch:
  hosts: ["elasticsearch:9200"]
  indices:
    - index: "nginx_access"

logging.json: true
logging.metrics.enabled: false
```