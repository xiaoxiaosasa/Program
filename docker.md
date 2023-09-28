# docker

## docker 工具使用

### 在容器中启动mysql并在外部连接mysql

1. docker desktop安装配置，安装完成之后可在cmd敲docker --version 查看是否安装成功

2. 拉取镜像，可在docker desktop可视化界面中拉取，也可以用docker pull 拉取

3. 启动容器

   ```shell
   docker run --restart=always --name mysql --privileged=true -d -p 3307:3306 -v D:\Docker\WorkSpace\MySQL\conf\my.cnf:/etc/mysql/my.cnf -v D:\Docker\WorkSpace\MySQL\logs:/logs -v D:\Docker\WorkSpace\MySQL\data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0.29
   
   
   # 参数解释
   --restart=always    -> 开机启动容器,容器异常自动重启
   --name              -> 指定容器名称
   -d                  -> 以守护进程的方式启动容器
   -p                  -> 映射宿主机端口
   -v                  -> 映射配置文件、日志、数据
   -e                  -> 写入配置root密码
   --privileged=true   -> 设置特权级运行的容器：使容器内的root拥有真正的root权限，否则container内的root只是外部的一个普通用户权限，很多操作会受限
   ```

   

4. 配置root远程连接权限

   ```shell
   cmd:
   	# 进入docker
   	docker exec -it mysql /bin/bash
   	# 连接数据库
   	mysql -uroot -p123456
   	# 配置支持远程连接
   	ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
   	# 刷新数据库
   	flush privileges;
   
   ```

   

5. 验证连接

   客户端连接验证 localhost 3307  ， root  123456 