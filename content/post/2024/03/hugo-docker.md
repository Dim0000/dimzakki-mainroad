---
title: 【Hugo】Docker上でHugo環境を構築する【Compose Watch】
description: 今回は、Hugo環境をDocker上で構築・実行する手順についてまとめました。
date: 2024-03-10
categories: 
  - 技術記事
tags: 
  - Hugo
  - ブログ運営
  - Docker
archives:
    - 2024/03
thumbnail: /images/hugo.webp
---

今回は、**Hugo**環境を**Docker**上で構築・実行する手順についてまとめました。

<!--more-->

{{< box "関連記事" >}}
<ul>
<li>{{< ref "/develop-hugo" >}}</li>
</ul>
{{< /box >}}

## DockerでHugo環境を構築する

Dockerを使うことで、`hugo.exe`のダウンロードや環境変数の設定をしなくてもHugoを動かせます（Dockerの環境構築については割愛します）。

ブログ用フォルダ内で`Dockerfile`と`docker-compose.yml`を配置します。ホットリロード機能を実現するために`Compose Watch`を利用しています。

{{< code lang="Dockerfile" title="Dockerfile" >}}
FROM hugomods/hugo:latest

COPY . /src/

EXPOSE 1313

ENTRYPOINT ["hugo", "server", "--bind", "0.0.0.0", "--port", "1313", "-D", "-F"]
{{< /code >}}

{{< code lang="yml" title="compose.yml" >}}
version: '3'

services:
  hugo:
    build: .
    ports:
      - "1313:1313"
    develop:
      watch:
        - action: sync
          path: .
          target: /src
{{< /code >}}

コンテナ起動時は、ターミナルで以下のコマンドを実行します。

{{< code lang="powershell" title="ターミナル" >}}
$ docker compose up -d # コンテナ起動・ローカルでビルド

$ docker compose watch # ホットリロード
{{< /code >}}

ローカルでの構築時と同様に、`http://localhost:1313/`を開くことでサイトプレビューを確認できます。

新規記事を作成する際は以下のコマンドを実行します。

{{< code lang="powershell" title="ターミナル" >}}
$ docker compose exec hugo hugo new <file> # コンテナ内に新規ファイル作成

$ docker compose cp hugo:/src/content/<file> . # コンテナからローカルにコピー
{{< /code >}}

* * *

今回は、Dockerでブログを構築する方法について紹介しました。ローカルにはブログのソースのみ置けば良いというのがDockerの利点ですね。以上で記事を終わりにします。

## 参考文献

* [Use Compose Watch | Docker Docs](https://docs.docker.com/compose/file-watch/)

* [Rerender on file change in Docker setup - support - HUGO](https://discourse.gohugo.io/t/rerender-on-file-change-in-docker-setup/45439)