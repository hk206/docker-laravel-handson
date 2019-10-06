Docker本より
```
FROM golang:1.9
# dockerのimageを指定する。
# デフォルトでdocker hubを参照する。
# image名の後に:タグ名とすると認識しやすい。
# 慣習的にはバージョンを書く

RUN mkdir /echo
# イメージビルドするときにコンテナ内で実行されるコマンド
# ここではechoディレクトリをコンテナ内に作る。
COPY main.go /echo
#　ホストマシン上のファイルやディレクトリをコンテナ内にコピーする。ここではmain.goをコンテナ内のechoディレクトリにコピーする。

CMD ["go", "run", "/echo/main.go"]
# コンテナとして実行する際にコンテナ内で実行するコマンド
# シェルで書くとgo run /echo/main.goとなる
# CMDではこれを分割し、配列にした形式で指定する
```

Dockerイメージの作成
```
docker image build -t イメージ名[:タグ名] Dockerfile配置ディレクトリのパス
```
これはDockerfileを元にイメージを作成するためのコマンド。
-tはオプションでこの後に任意のイメージ名を指定する。タグも指定でき、指定しなければlatestとなる。
これはイメージ名をつけないとイメージをハッシュ値で管理することになるため。
デフォルトでdockerfile探すが違うなら-f

```html:例
docker image build -t example/echo:latest .
```

dockerイメージはcontainerのテンプレート
dockerfileはイメージを構築するための手順

containerには作成後にIDがついて、かつコンテナ名っていう適当な単語をつなげたものがありどちらもコンテナを参照できるが、確認がめんどくさい。このコンテナ名だけは作成時に任意につけることができるのでつけ他方が良い。

すると完全体はこんな感じ
```
docker container run -t -d -p 9000:8080 --name hokuto example/echo:latest
```
nameにタグをつけ、バックグラウンドで実行し、ポートフォワーディングをあて、container名をつける。

コンテナーをビルとするとたくさんコンテナーが増えたけど決してまたそれ使いたいならrestartを使うこと
docker stop kill rm とか色々あっる


