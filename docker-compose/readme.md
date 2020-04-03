■起動
```
docker-compose up -d
```

■停止＆コンテナ削除
```
docker-compose down
```

■証明書作る用
```
docker-compose -f create-certs.yml run --rm create_certs
```

■動作確認
```
docker run --rm -v es_certs:/certs --network=es_default docker.elastic.co/elasticsearch/elasticsearch:7.6.2 curl --cacert /certs/ca/ca.crt -u elastic:PleaseChangeMe https://127.0.0.1:9200
```
