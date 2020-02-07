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
mkdir -p /data/cowrie/log
mkdir -p /data/dionaea/log

貼り付け元  <https://qiita.com/okcoder/items/00c60614b819edc0c0b8> 
```

■logstash設定ファイル
```
#input section
Input{
	#cowrie
	File{
		Path => "/data/cowrie/log/cowrie.json"
		Codec => json
		Type => "Cowrie"
	}
	#dionaea
	File{
		Path => "/data/dionaea/log/dionaea.json"
		Codec => json
		Type => "Dionaea"
	}
}

#filter section
Filter{
	#cowrie
	If [type] == "Cowrie" {
		Date{
			Match => ["timestamp", "ISO8601"]
		}
		Mutate{
			Rename => {
				"dst_port" => "dest_port"
				"dst_ip" => "dest_ip"
			}
		}
	}
	#dionaea
	if [type] == "Dionaea" {
		date {
			match => [ "timestamp", "ISO8601" ]
		}
		mutate {
			rename => {
				"dst_port" => "dest_port"
				"dst_ip" => "dest_ip"
			}
			gsub => [
				"src_ip", "::ffff:", "",
				"dest_ip", "::ffff:", ""
			]
		}
		if [credentials] {
			mutate {
				add_field => {
					"username" => "%{[credentials][username]}"
					"password" => "%{[credentials][password]}"
				}
				remove_field => "[credentials]"
			}
		}
	}
}
#output section
Output{
	Elasticsearch{
		Hosts => ['elasticsearch"]
	}
}

```
