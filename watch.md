■ logstash 確認
```
watch -n 1 curl -u elastic:pasword -XGET "http://localhost:9200/logstash/_count?pretty"
```
