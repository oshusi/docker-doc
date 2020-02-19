■セキュリティの有効化
1. /usr/share/elasticsearch/config/elasticsearch.yml に設定を追加
```
cluster.name: "docker-cluster"
network.host: 0.0.0.0
# 次の1行をadd 
xpack.security.enabled: true
```
2. コンテナを再起動し、/usr/share/elasticsearch/bin/elasticsearch-setup-passwordsを実行
```
$ ./usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
```
→それぞれのパスワードを設定
3. http://xx.xx.xx.xx:9200/にアクセスして、ログイン画面が表示されることを確認
4. /usr/share/kibana/config/kibana.yml に設定を追加
```
#
# ** THIS IS AN AUTO-GENERATED FILE **
#

# Default Kibana configuration for docker target
server.name: kibana
server.host: "0"
elasticsearch.hosts: [ "http://elasticsearch:9200" ]
xpack.monitoring.ui.container.elasticsearch.enabled: true


# この２行を追加する
elasticsearch.username: kibana
elasticsearch.password: 設定したパスワード
```
5. http://xx.xx.xx.xx:5601/にアクセスしてログイン画面が表示されることを確認
→ログインユーザはelasticになることに注意
