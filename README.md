# JTF2018

JTF2018デモ用

<a href="http://play-with-docker.com?stack=https://raw.githubusercontent.com/mnagaku/jtf2018/master/docker-compose-from-github.yml"><img src="https://raw.githubusercontent.com/play-with-docker/stacks/master/assets/images/button.png" /></a>

※　上の「Try in PWD」を　[ctrl]+クリック　すると、クラウド上にデモ環境が準備されるので、ブラウザだけで試すことができます。ローカルのdocker環境で試す場合は、　git clone　してから　$Env:COMPOSE_CONVERT_WINDOWS_PATHS=1　して　docker-compose up -d　します

----

## 作業環境の準備

Ansibleで操作対象となるサーバがdockerコンテナなので、Jupyterから使えるdockerコマンドをインストールし、操作対象となるコンテナをインベントリファイルに登録します

[00_作業環境の準備](00_作業環境の準備.ipynb)

----

## デモシナリオ（ターミナルでの作業）

### 2018-07-25_3dwebmapの調査

M先輩から「3dwebmapのサーバ、中で何動いてるかレポート作って」と指示があった

1. ターミナルのログを残す設定にする
2. sudo docker exec -it jtf2018_3dwebmap_1 bash （sshの代わり）
3. 「linux os 確認」でググる、/etc/issueを見つける
4. cat /etc/issue （OSがdebian8と確認）
5. 「debian サービス 一覧」でググる、insservを見つける
6. insserv -s （サービス一覧を表示、それっぽいサービスは見つからず）
7. ps aux （プロセスを確認、node.jsが動いてるのを見つける）
8. 「listen debian」でググる、ssコマンドを見つける
9. ss -lnat ; ss -lnau （8000番が開いてるのを確認する）
10. ブラウザで8000番を確認するとwebアプリが動いてるのが分かる

----

## デモシナリオ（LC4RI）

### 2018-07-25_3dwebmapの調査

M先輩から「3dwebmapのサーバ、中で何動いてるかレポート作って」と指示があった

Python3(LC_wrapper)の新しいノートブックを作り、ファイル名などを付ける

ログ保存を有効化
```
%env lc_wrapper_force=on
```

#### 調査対象の確認

ansibleのインベントリファイルで対象サーバを確認し、簡単なコマンドが通ることを確認しておく

```
hosts = !cat ./hosts
hosts
```

```
for h in hosts:
        if h.find('3dwebmap') > -1:
            target = ' -i ./hosts {} -c docker'.format(h)
target
```

```
!sudo ansible -a "ls -la" {target}
```

#### 調査対象から情報を収集

##### OSのバージョン

「linux os 確認」でググって見つけた、/etc/issueを確認する方法を使う

https://eng-entrance.com/linux-os-version

```
command = "'cat /etc/issue'"
!sudo ansible -a {command} {target}
```

##### サービス

「debian サービス 一覧」でググって見つけた、insservを使う

https://www.mk-mode.com/octopress/2015/06/03/debian-8-service-management/

```
command = "'insserv -s'"
!sudo ansible -a {command} {target}
```

webサーバなどの、それっぽいサービスは見つからず

##### プロセス

psコマンド、これは覚えてる

```
command = "'ps aux'"
!sudo ansible -a {command} {target}
```

node.jsが動いてる

##### LISTENポート

「listen debian」でググって見つけた、ssを使う

http://mzgkworks.com/post/linux-port-confirm/

```
command = "'ss -lnat'"
!sudo ansible -a {command} {target}
```

```
command = "'ss -lnau'"
!sudo ansible -a {command} {target}
```

8000番ポートでLISTENしてる

#### サービスの確認

8000番ポートにブラウザでアクセスしてみる

画面キャプチャを貼る

地図が見れるwebアプリだった

#### まとめ

node.jsで8000番ポートからサービスされてる、地図が見れるwebアプリが動いている

### 2018-07-26_wikiの調査

M先輩から「wikiのサーバ、中で何動いてるかレポート作って」と指示があった

「2018-07-25_3dwebmapの調査」とほぼ同じ作業が予想されたので、後輩に任せることにした

ノートブックをコピーして、ファイル名などを整えて、適時書き換えながら作業を行うこととする

##### サービス

流していくと、insservが使えないとこで詰まる

「ubuntu サービス 確認」でググって、何種類か載ってるのを見つけたので、順に試す

https://server-setting.info/debian/debian-like-chkconfig.html

```
command = "'sysv-rc-conf --list'"
!sudo ansible -a {command} {target}
```

```
command = "'rcconf --list'"
!sudo ansible -a {command} {target}
```

```
command = "'ls /etc/rc1.d/'"
!sudo ansible -a {command} {target}
command = "'ls /etc/rc2.d/'"
!sudo ansible -a {command} {target}
command = "'ls /etc/rc3.d/'"
!sudo ansible -a {command} {target}
command = "'ls /etc/rc4.d/'"
!sudo ansible -a {command} {target}
command = "'ls /etc/rc5.d/'"
!sudo ansible -a {command} {target}
```

webサーバなどの、それっぽいサービスは見つからず

##### プロセス

/home/wiki/realms-wiki 以下にある実行ファイルが、pythonで動いてる雰囲気

##### LISTENポート

5000番ポートでLISTENしてる

#### サービスの確認

5000番ポートにブラウザでアクセスしてみる

画面キャプチャを貼る

「realms-wiki」でググってみたところ

https://github.com/scragg0x/realms-wiki

wikiのようだ

#### まとめ

5000番ポートからサービスされてるpython製のwiki「realms」が動いている

