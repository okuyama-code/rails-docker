# 本ソースコードをDocker化するための手順

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

