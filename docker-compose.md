```
cd ~/dev/happiness/rails-docker && code .
```

```Dockerfile
version: "3.9"

# Docker Composeで使用するボリュームを定義します。
volumes:
  db-data:  # db-dataという名前のボリュームを作成します。

# サービスを定義します。
services:

  # Webサービス
  web:
    build: .  # カレントディレクトリからのビルドを指定します。
    command: bundle exec rails s -p 3000 -b '0.0.0.0'  # Railsサーバーの起動コマンドを指定します。
    ports:
      - '3000:3000'  # ホストマシンのポート3000とコンテナのポート3000をマッピングします。
    volumes:
      - '.:/rails-docker'  # ホストマシンのカレントディレクトリをコンテナの/rails-dockerにマウントします。
    environment:
      - 'DATABASE_PASSWORD=postgres'  # データベースのパスワードを環境変数として設定します。
    tty: true
    stdin_open: true
    depends_on:
      - db  # dbサービスが起動するまで待機します。

  # データベースサービス
  db:
    image: postgres:12  # PostgreSQL 12の公式イメージを使用します。
    volumes:
      - 'db-data:/var/lib/postgresql/data'  # データベースのデータを永続化するためのボリュームをマウントします。
    environment:
      - 'POSTGRES_USER=postgres'  # PostgreSQLのユーザー名を設定します。
      - 'POSTGRES_PASSWORD=postgres'  # PostgreSQLのパスワードを設定します

```
