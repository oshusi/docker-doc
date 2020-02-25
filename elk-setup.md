■環境
```
#Dockerに共有するフォルダを作成
sudo mkdir -p /data/elk
sudo chown user:user /data/elk
#elasticsearch用共有フォルダ作成
mkdir /data/elk/elasticsearch
#logstash用共有フォルダ作成
mkdir -p /data/elk/logstash/pipeline
#ハニポ用共有フォルダ作成
mkdir -p /data/elk/log/cowrie
mkdir -p /data/elk/log/dionaea

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 
```

■logstash設定ファイル
```
input{
	file{
		path => "/data/elk/log/cowrie/cowrie.json"
		start_position => beginning
		codec => "json"
	}
}

#filter section ★今はtype非推奨のため、multiple-pipelineに変更予定
filter{
	date{
		match => ["timestamp", "ISO8601"]
	}
}

output{
	elasticsearch{
		hosts => ['elasticsearch:9200']
		index => "logstash-%{+YYYY.MM.dd}"
	}
}

★outputのhostsがelasticsearchなのはコンテナの名前がそうなるから
```

■認証局用
```
sudo docker run -d --restart always -v /data/elk/certificate:/usr/share/elasticsearch/certificate  -e "discovery.type=single-node"  --name ca docker.elastic.co/elasticsearch/elasticsearch:7.5.2
```

■起動時のコマンド
```
sudo docker run -d -v /data/elk/log/cowrie:/cowrie/cowrie-git/var/log/cowrie --restart always -p 22:2222 cowrie/cowrie

貼り付け元  <https://github.com/cowrie/cowrie> 


sudo docker run -d -v /data/elk/log/dionaea:/opt/dionaea/var/log --restart always -p 21:21 -p 42:42 -p 69:69/udp -p 80:80 -p 135:135 -p 443:443 -p 445:445 -p 1433:1433 -p 1723:1723 -p 1883:1883 -p 1900:1900/udp -p 3306:3306 -p 5060:5060 -p 5060:5060/udp -p 5061:5061 -p 11211:11211 my-dionaea

貼り付け元  <https://github.com/DinoTools/dionaea-docker> 


sudo docker run -d --restart always -v /data/elk/elasticsearch:/usr/share/elasticsearch/data -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node"  --name elasticsearch docker.elastic.co/elasticsearch/elasticsearch:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 

sudo docker run -d --restart always -v /data/elk/logstash/pipeline:/usr/share/logstash/pipeline -v /data/elk/log:/var/log --link elasticsearch:elasticsearch  --name logstash docker.elastic.co/logstash/logstash:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 

sudo docker run -d --restart always -p 5601:5601 --link elasticsearch:elasticsearch -e ELASTICSEARCH_URL=http://elasticsearch:9200 --name  kibana docker.elastic.co/kibana/kibana:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8>
```


■テスト時
```
sudo docker run -v /data/elk/log/cowrie:/cowrie/cowrie-git/var/log/cowrie --restart always -p 2222:2222 cowrie/cowrie

貼り付け元  <https://github.com/cowrie/cowrie> 


sudo docker run -v /data/elk/log/dionaea:/opt/dionaea/var/log --restart always -p 21:21 -p 42:42 -p 69:69/udp -p 80:80 -p 135:135 -p 443:443 -p 445:445 -p 1433:1433 -p 1723:1723 -p 1883:1883 -p 1900:1900/udp -p 3306:3306 -p 5060:5060 -p 5060:5060/udp -p 5061:5061 -p 11211:11211 my-dionaea

貼り付け元  <https://github.com/DinoTools/dionaea-docker> 


sudo docker run --restart always -v /data/elk/elasticsearch:/usr/share/elasticsearch/data -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node"  --name elasticsearch docker.elastic.co/elasticsearch/elasticsearch:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 

sudo docker run --restart always -v /data/elk/logstash/pipeline:/usr/share/logstash/pipeline -v /data/elk/log:/var/log --link elasticsearch:elasticsearch  --name logstash docker.elastic.co/logstash/logstash:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 

sudo docker run --restart always -p 5601:5601 --link elasticsearch:elasticsearch -e ELASTICSEARCH_URL=http://elasticsearch:9200 --name  kibana docker.elastic.co/kibana/kibana:7.5.2

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8>
```
