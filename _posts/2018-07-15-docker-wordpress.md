
---

tags: Wordpress
tags: docker

---

# Docker 建立 wordpress 的紀錄

紀錄一下用 docker 建立 wordpress 的過程。下列是拿現用的站建立的，並沒有試過直接建一個全新的 wordpress ，需要的請再自行嘗試。

## 1. 建立 mysql 

首先建立一個 mysql 的 docker ，這邊要注意不能用最新的 8 的版本，所以會這樣下：

```
docker run --name {my-mysql} -e MYSQL_ROOT_PASSWORD={MYSQLPASSWORD} -d -p 3306:3306 mysql:5.7
```

其中 `{my-mysql}` `{MYSQLPASSWORD}` 分別填入自訂的 container 的名稱和你設的 root 的密碼。

建好之後，將來要登入 mysql 可用這個指令

```
docker exec -it {my-mysql} drush -uroot -p{MYSQLPASSWORD}
```

## 2.建立資料庫

進入後下這個 sql 語法來建立資料庫：

```sql
create database {database_name_user};
create user {database_name_user}@'%' identified by '{password}';grant all privileges on {database_name_user}.* to  {database_name_user}@'%';
```

這邊我把 `{database_name_user}` 同時當使用者和資料表名稱， `{password}` 填入此使用者的密碼

然後因為我要匯入備份的資料庫檔案，我會先在主機把檔案複製到 container 中，然後做匯入的動作

```
docker cp {sql_file} {my-mysql}:/
```

```
docker exec -it {my-mysql} drush -uroot -p{MYSQLPASSWORD} < /{sql_file}
```

## 3. 建立 wordpress container

docker 的 wordpress 可以參考[它的頁面](https://hub.docker.com/_/wordpress/)

我是用 php7-apache 的版本

```
docker run --name {site_name} \
-e WORDPRESS_DB_USER={database_name_user} \
-e WORDPRESS_DB_NAME={database_name_user} \
-e WORDPRESS_DB_PASSWORD={password} \
-v /path/to/folder:/var/www/html \
--link {my-mysql}:mysql \
-p 30000:80 wordpress:php7.0-apache
```

這樣就可以用 port 30000 連進去了，試試看吧

## 備註

### 1. 除錯方法
試過如果 mysql 用 latest 會沒辦法執行，會出現這個錯誤訊息，詳情可參考[這個](https://github.com/docker-library/wordpress/issues/313)：

```
MySQL Connection Error: (2054) The server requested authentication method unknown to the client
```

會找到這個是因為拿掉 `docker run ` 中的 `-d` 參數，就能看到執行時的訊息。