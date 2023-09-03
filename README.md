# 本ソースコードをDocker化するための手順
https://github.com/okuyama-code/rails-docker
## 1.以下のURLからテンプレートを自分のリポジトリにも作る
https://github.com/ihatov08/rails7_docker_template

Use this templateボタンを押しでCreate a new repositoryを選択し、自分のアカウント配下にリポジトリを作成。rails-dockerという名前のリポジトリにする。

## 2. git cloneしてテンプレートを確認
クローンしたいディレクトリに移動
```
cd ~/dev/happiness/ && ls
```
```
git clone https://github.com/okuyama-code/rails-docker.git
```
```
 cd rails-docker/
 code .
```
サーバーを起動する(自分の場合bashではなくpawershell)
```
bundle install
rails s
```
```
ActiveRecord::ConnectionNotEstablished
connection to server at "localhost" (::1), port 5432 failed: fe_sendauth: no password supplied
```
このエラーが出るということはサーバーがたてられている。

## dokerブランチの作成し移動
```
git checkout -b docker
```
## 3.docker化していく
Dockerfile作成
```Dockerfile
FROM ruby:3.2.2
RUN apt-get update -qq && apt-get install -y \
  build-essential \
  libpq-dev \
  nodejs \
  postgresql-client \
  yarn
WORKDIR /rails-docker
COPY Gemfile Gemfile.lock /rails-docker/
RUN bundle install
```

Docker-compose.yml作成

```Dockerfile
version: "3.9"

volumes:
  db-data:

services:
  web:
    build: .
    ports:
      - '3000:3000'
    volumes:
      - '.:/rails-docker'
    environment:
      - 'DATABASE_PASSWORD=postgres'
    tty: true
    stdin_open: true
    depends_on:
      - db
    links:
      - db

  db:
    image: postgres:12
    volumes:
      - 'db-data:/var/lib/postgresql/data'
    environment:
      - 'POSTGRES_USER=postgres'
      - 'POSTGRES_PASSWORD=postgres'

```
Dockerアプリを起動しコマンド入力
```
docker-compose up -d
```
