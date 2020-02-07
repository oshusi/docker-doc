■Cowrie
```
sudo docker pull cowrie/cowrie
```

■Dionaea
```
git clone https://github.com/Dinotools/dionaea-docker
cd 0.6
sudo docker build . -t my-dionaea
```

■ELKコンテナ
```
sudo docker pull docker.elastic.co/elasticsearch/elasticsearch:7.5.2

貼り付け元  <https://www.docker.elastic.co/#> 

sudo docker pull docker.elastic.co/kibana/kibana:7.5.2

貼り付け元  <https://www.docker.elastic.co/#> 

sudo docker pull docker.elastic.co/logstash/logstash:7.5.2

貼り付け元  <https://www.docker.elastic.co/#> 
```

■起動時のコマンド
```
sudo docker run -d --restart always -p 2222:2222 cowrie/cowrie

貼り付け元  <https://github.com/cowrie/cowrie> 


sudo docker run --rm -it -p 21:21 -p 42:42 -p 69:69/udp -p 80:80 -p 135:135 -p 443:443 -p 445:445 -p 1433:1433 -p 1723:1723 -p 1883:1883 -p 1900:1900/udp -p 3306:3306 -p 5060:5060 -p 5060:5060/udp -p 5061:5061 -p 11211:11211 my-dionaea

貼り付け元  <https://github.com/DinoTools/dionaea-docker> 


sudo docker run -d --restart always -v /data/elk/elasticsearch:/usr/share/elasticsearch/data -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node"  --name elasticsearch docker.elastic.co/elasticsearch/elasticsearch:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 

sudo docker run -d --restart always -v /data/elk/logstash/pipeline:/usr/share/logstash/pipeline -v /data/elk/log:/var/log --link elasticsearch:elasticsearch  --name logstash docker.elastic.co/logstash/logstash:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 

sudo docker run -d --restart always -p 5601:5601 --link elasticsearch:elasticsearch -e ELASTICSEARCH_URL=http://elasticsearch:9200 --name  kibana docker.elastic.co/kibana/kibana:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8>
```
