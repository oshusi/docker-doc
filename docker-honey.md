■Cowrie
```
sudo docker pull cowrie/cowrie
```

■Dionaea
```
vm Dockerfile
FROM dinotools/dionaea:latest
COPY conf/your-service.yaml /opt/dionaea/etc/dionaea/services-enabled/
COPY conf/your-ihandler.yaml /opt/dionaea/etc/dionaea/ihandlers-enabled/
sudo docker build . -t my-dionaea
```

■ELKコンテナ
```
sudo docker pull docker.elastic.co/elasticsearch/elasticsearch:7.6.2

貼り付け元  <https://www.docker.elastic.co/#> 

sudo docker pull docker.elastic.co/kibana/kibana:7.6.2

貼り付け元  <https://www.docker.elastic.co/#> 

sudo docker pull docker.elastic.co/logstash/logstash:7.6.2

貼り付け元  <https://www.docker.elastic.co/#> 
```

