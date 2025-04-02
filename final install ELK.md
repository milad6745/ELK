```
git clone https://github.com/deviantony/docker-elk.git
docker-compose setup
docker-compose up -d
```
check:
```
curl -s http://127.0.0.1:9200 -u elastic:changeme
```
```
docker exec -it docker-elk_elasticsearch_1  /bin/bash
bin/elasticsearch-reset-password -u kibana_system
y
New value: 4=3R4=uboSB+YFN5iTuT
```
```
nano /home/milad/docker-elk/kibana/config/kibana.yml
elasticsearch.password: 4=3R4=uboSB+YFN5iTuT
```
```
docker-compose restart
```

go to kibbana web
```
http:127.0.0.1:5601
user:elastic
pass:changeme
for change:docker  exec -it docker-elk_elasticsearch_1  /bin/bash
#bin/elasticsearch-reset-password --batch --user elastic
```
