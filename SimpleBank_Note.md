# Note

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

**run command in container**

`docker exec -it <container_name_or _id> <command> [args]`

`-it`告诉docker将命令作为交互式TTY会话运行

**view container logs**

`docker logs <docker_name_or_id>`

### Table Plus





