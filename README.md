# docker-rails-mysql
rails and mysql with docker-compose with data volume container

## 構成

* web      : railsが起動するコンテナ
* db       : mysqlが起動するコンテナ
* db_data  : dbの/var/lib/mysql

### railsのセットアップ

Gemfileに`gem 'mysql2', '~> 0.3.20'`が記載されているのは、ruby2.3.0とRails5.0.0で組み合わせで検証した際、アプリを起動するとmysql2が読み込まれないという状況に陥ったため仕方なく3.2系を明示しています

```
$ docker-compose build
$ docker-compose run web rails new . --force --database=mysql
```

#### config/database.ymlを編集

```
default: &default
   adapter: mysql2
   encoding: utf8
   pool: 5
   username: root
   password: testpassword
   host: db
```
#### db:create
```
$ docker-compose build
$ docker-compose run web rake db:create
Starting dockerrailsmysql_db_data_1
Created database 'myapp_development'
Created database 'myapp_test'
```

### 環境の起動

```
$ docker-compose up -d
```

### 動作確認

```
$ docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED              STATUS              PORTS                    NAMES
790bbfa66271        dockerrailsmysql_web   "bundle exec rails s "   About a minute ago   Up 59 seconds       0.0.0.0:3000->3000/tcp   dockerrailsmysql_web_1
4dabf780ecbf        mysql:5.7              "docker-entrypoint.sh"   14 minutes ago       Up 7 minutes        3306/tcp                 dockerrailsmysql_db_1
```

ブラウザで`localhost:3000`を開けばwelcomeページが出ていると思います

終了させるには

```
$ docker-compose stop
```
