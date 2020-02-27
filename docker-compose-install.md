```
export compose='1.25.1'
sudo curl -L https://github.com/docker/compose/releases/download/${compose}/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod 0755 /usr/local/bin/docker-compose
docker-compose -v
```
https://qiita.com/iganari/items/fe4889943f22fd63692a
