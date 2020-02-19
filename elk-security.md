■セキュリティの有効化、パスワード設定
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

■Kibanaへの通信の暗号化
1. 認証局を発行する
```
# <ES_HOME>/bin/elasticsearch-certutil ca
Please enter the desired output file [elastic-stack-ca.p12]: <任意のファイル名もしくは空白>
Enter password for elastic-stack-ca.p12 : <任意のパスワードもしくは空白>
```
2. 証明書と秘密鍵を出力する
```
$ bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12 --pem --name kibana
```
3. 出力されたcertificate-bundle.zipをホスト側へコピーし、権限を変更する
```
sudo chown user:user certificate-bundle.zip
```
4. unzipを行い、秘密鍵のアクセス権変更を行う
```
unzip certificate-bundle.zip
cd kibana
sudo chmod 660 kibana.key
```
5. kibana.key、kibana.certをコンテナ内に移動し、kibana.ymlに以下のように追記し再起動を行う
```
server.ssl.enabled: true
server.ssl.key: /etc/kibana/kibana.key
server.ssl.certificate: /etc/kibana/kibana.crt
```

