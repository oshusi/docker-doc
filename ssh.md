■sshのinstall
```
sudo apt-get install openssh-server
```

■sshのセキュリティ設定
```
ポートの設定は「/etc/ssh/sshd_config」にて行います。Ubuntuにて、「vi /etc/ssh/sshd_config」として、編集していきましょう。
`# Port 22`となっているところがあるので、コメントアウトして、1-65535の中で好きなものを設定しましょう。

root権限の無効は、PermitRootLoginをコメントアウトし、noにしましょう。
```

■システムの再起動とか
```
sudo service ssh start
```
