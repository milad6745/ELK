## install logstash
```
version: "3.7"

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.6.0
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - ELASTICSEARCH_USERNAME=elastic
      - ELASTICSEARCH_PASSWORD=milad123
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - esdata:/usr/share/elasticsearch/data
    networks:
      - elastic_network

  kibana:
    image: docker.elastic.co/kibana/kibana:8.6.0
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
    networks:
      - elastic_network

  logstash:
    image: docker.elastic.co/logstash/logstash:8.6.0
    container_name: logstash
    volumes:
      - ./logstash/config:/usr/share/logstash/config
      - ./logstash/pipeline:/usr/share/logstash/pipeline
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    ports:
      - "5044:5044"  # برای دریافت لاگ از Filebeat
      - "9600:9600"  # پورت API
    depends_on:
      - elasticsearch
    networks:
      - elastic_network

volumes:
  esdata:
    driver: local

networks:
  elastic_network:
    driver: bridge
```


## create file and directory
```
mkdir /logstash/config/logstash.yml  
mkdir /logstash/config/pipelines.yml
cat logstash.yml 
http.host: "0.0.0.0"
xpack.monitoring.enabled: true
xpack.monitoring.elasticsearch.hosts: [ "http://elasticsearch:9200" ]

cat pipelines.yml 
- pipeline.id: main
  path.config: "/usr/share/logstash/pipeline/logstash.conf"

```

  ## dosable auth on Elastik
```
docker exec -it elasticsearch /bin/bash
sed -i 's/xpack.security.enabled: true/xpack.security.enabled: false/' /usr/share/elasticsearch/config/elasticsearch.yml
OR
echo "xpack.security.enabled: false" >> /config/elasticsearch.yml
```
