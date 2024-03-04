# dockNote

## 功能

- 创建管理账户 Create and manage account

  Owner, balance, currency

- 记录所有收支变化 Record all balance changes

  Create an account entry for each change

- 转账 Money transfer transaction

  Perform money transfer between 2 accounts consistently within a transaction

## Design DB schema

> 辅助工具：[DB Diagram](https://dbdiagram.io/home)
>
> 数据库：PostgreSQL

1. 设计数据库架构 Design DB schema
2. 保存数据库图 Save & share DB diagram
3. 生成SQL代码 Generate SQL code

结果：https://dbdiagram.io/d/Simple-Bank-65dc8ed65cd0412774d40621

## Install DevTools

> docker
>
> Postgres

### Docker

`docker pull <image>:<tag>`

**start a container**

`docker run --name <container_name> -e <environment_variable> -p <host_posts:container_posts> -d <image>:<tag>`

`-d`:在后台运训

**run command in container**

`docker exec -it <container_name_or _id> <command> [args]`

```
docker exec -it postgres16 psql -U root
```

`-it`：这是两个选项的组合。`-i` 表示交互式操作，`-t` 表示分配一个伪终端（pseudo-TTY），这两个选项一起允许您与正在执行的命令进行交互。

**view container logs**

`docker logs <docker_name_or_id>`

### Table Plus

> DataGrip 替代

## Write & run DB migration by Golang

>  [golang-migrate](https://github.com/golang-migrate/migrate)

```
Commands:
	create [-ext E] [-dir D] [-seq] [-digits N] [-format] NAME
               Create a set of timestamped up/down migrations titled NAME, in directory D with extension E.
               Use -seq option to generate sequential up/down migrations with N digits.
               Use -format option to specify a Go time format string.
    goto V       Migrate to version V
    up [N]       Apply all or N up migrations
    down [N]     Apply all or N down migrations
```

1. 创建新文件夹(`db/migration`)存储所有的迁移文件

2. 创建第一个迁移文件，初始化simplebank的数据库模式

   ```
   >migrate create -ext sql -dir db\migration -seq init_schema
   .\db\migration\000001_init_schema.up.sql
   .\db\migration\000001_init_schema.down.sql
   
   -ext sql: 给定了文件的扩展名
   -dir db\migration: 指定创建迁移文件的目录
   -seq: 创建一组带有序列数字的迁移文件
   init_schema: 文件名会以 "init_schema" 命名
   up.sql: 进行数据库迁移时运训up-script对模式进行向前更改，依次执行up.sql
   down.sql: 恢复up-script所做的更改，则运行down-script
   ```

3. 将`SimpleBank_PostgreSQL.sql`的内容复制到`000001_init_schema.up.sql`

4. 对于`000001_init_schema.down.sql`，恢复up-script所做的更改

   ```sql
   DROP TABLE IF EXISTS entries;
   DROP TABLE IF EXISTS transfers;
   DROP TABLE IF EXISTS accounts;
   // 存在外键约束，先删除entries,transfers;再删除accounts
   ```

**docker 操作 postgresql**

`docker exec -it postgres16 /bin/sh`

**创建数据库**

`createdb --username=root --owner=root simple_bank`

通过docker操作：`docker exec -it postgres16 createdb --username=root --owner=root simple_bank`

**利用migrate迁移数据库**

`migrate -path db/migration -database "postgresql://root:password@127.0.0.1:5432/simple_bank?sslmode=disable" -verbose up`

**集成到Makefile**

```makefile
postgres:
	docker run --name postgres16 -p 5432:5432 -e POSTGRES_USER=root -e POSTGRES_PASSWORD=password -d postgres:16-alpine

createdb:
	docker exec -it postgres16 createdb --username=root --owner=root simple_bank

dropdb:
	docker exec -it postgres16 dropdb simple_bank

migrateup:
	migrate -path db/migration -database "postgresql://root:password@127.0.0.1:5432/simple_bank?sslmode=disable" -verbose up

migratedown:
	migrate -path db/migration -database "postgresql://root:password@127.0.0.1:5432/simple_bank?sslmode=disable" -verbose down

.PHONY: postgres createdb dropdb migrateup migratedown
```







