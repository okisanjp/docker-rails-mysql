# docker-rails-mysql
rails and mysql with docker-compose with data volume container

## 構成

* web      : railsが起動するコンテナ
* db       : mysqlが起動するコンテナ
* db_data  : dbの/var/lib/mysql
