■sshのinstall
```
sudo apt-get install openssh-server
```

■sshのセキュリティ設定
```
ポートの設定は「/etc/ssh/sshd_config」にて行います。Ubuntuにて、「vi /etc/ssh/sshd_config」として、編集していきましょう。
【# Port 22】となっているところがあるので、コメントアウトして、1-65535の中で好きなものを設定しましょう。

root権限の無効は、PermitRootLoginをコメントアウトし、noにしましょう。
```

■システムの再起動とか
```
sudo service ssh start
```

■公開鍵認証
①秘密鍵と公開鍵の作成
```
$ ssh-keygen -t ecdsa -b 521

Generating public/private ecdsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_ecdsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```
→秘密鍵：id_ecdsa（クライアント側に保存）と公開鍵：id_ecdsa.pub（サーバ側に保存）が作成される

②公開鍵をサーバへ移動、ログインするユーザのホームディレクトリの.sshディレクトリに鍵の名前を変えて保存
```
mkdir .ssh
chmod 700 .ssh
cp ip_ecdsa.pub ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

③パスワード認証の無効
`/etc/ssh/sshd_config`の「#PasswordAuthentication yes」を「PasswordAuthentication no」に書き換え、サービスの再起動
