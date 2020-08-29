[TOC]

# Docker

Docker 属于 Linux 容器的一种封装。他将应用程序与该程序的依赖，打包在一个 image 文件（二进制文件）里面，通过运行模板文件生成一个虚拟容器。

Docker 根据 image 文件生成容器的实例，同一个image 通过可以生成多个同时运行的容器实例。image 文件也可以互相继承。



## Dockerfile

### From 

```dockerfile
From node:8.4 as builder
# FROM <image>[:<tag>] [AS <name>]
# FROM <image>[@<digest>] [AS <name>]
```

`From` 指定基础镜像，tag 或 digest 可选，不使用时会使用 latest 版本的基础镜像，如果本地没有指定镜像，则会从 Docker 公共库 pull 下来

`as name` 是添加一个新的名称用于后续 `FROM` 和 `COPY --from=<name|index>` 引用此阶段构建的镜像



**多阶段构建multi-stage**

通常为了控制镜像体积大小，需要区分软件的编译环境和运行环境等环节，docker 允许设置多个 stage，每个`FROM`指令代表一个 stage 的开始部分，可以把一个 stage 的产物拷贝到另一个 stage 中，没有name 则默认以 index 索引来引用。上述例子中`as builder`则可以通过 `--from=builder` 在后续 stage 引用，否则可以使用`--from=0`



### MAINTAINER

提供维护者信息

```dockerfile
MAINTAINER <name>
# MAINTAINER Hetao <xxx@gmail.com>
```



### WORKDIR

指定当前工作目录 （相当于 cd)，可多次使用，如果后续命令的参数是相对路径，则会基于之前命令指定的路径

```dockerfile
WORKDIR /a  
# 这时工作目录为/a
WORKDIR b  
# 这时工作目录为/a/b
WORKDIR c  
# 这时工作目录为/a/b/c
```



### COPY

复制本地（dockerfile 所在目录的相对路径）到容器中，不能访问网络资源

```dockerfile
COPY . /app
# 将当前目录下的所有文件（除了 .dockerignore 排除的文件）都拷贝到容器文件的 /app 目录，也可以指定具体文件，比如
COPY package.json /app
```



### RUN

在镜像容器中执行命令

```dockerfile
# shell 终端运行命令 相当于执行/bin/sh -c "<command>"
RUN <command>
RUN npm install --registry=https://registry.npm.taobao.org
# exec 被解析为一个 JSON 数组
RUN ["executable", "param1", "param2"]
```

每条 `RUN` 指令将在当前镜像基础上执行指定命令，并提交为新的镜像，后续的 `RUN` 都在之前 `RUN` 提交后的镜像为基础，镜像是分层的，可以通过一个镜像的任何一个历史提交点来创建，类似源码的版本控制。

RUN 指令创建的中间镜像会被**缓存**，并会在下次构建中使用，如果不想使用这些缓存镜像，可以在构建时指定 --no-cache 参数



### EXPOSE

指定容器要暴露的端口号，供互联系统使用

```dockerfile
EXPOSE <port> [<port>...]
```

`EXPOSE`并不会让容器的端口访问到主机，要使其可访问，需要在 docker run 运行容器时通过 `-p` 或者 `-P` 来发布这些端口



### ENV

定义环境变量，可被后续 RUN 指令使用并在容器运行时保持

```dockerfile
ENV <key> <value> 
#<key>之后的所有内容均会被视为其<value>的组成部分，因此，一次只能设置一个变量
ENV <key>=<value>...
#可以设置多个变量，每个变量为一个"<key>=<value>"的键值对，如果<key>中包含空格，可以使用\来进行转义，也可以通过""来进行标示；另外，反斜线也可以用于续行
ENV myName John Doe
ENV myCat=fluffy
ENV PATH /usr/local/nginx/sbin:$PATH
```



### ADD

类似 copy，将本地文件添加到容器中，tar 类型文件会自动解压，也可以是一个 url 网络资源，不过网络压缩资源不会解压，src 支持正则模糊匹配

```dockerfile
ADD <src>...<dest>
# dest 必须是绝对路径，若不存在，则自动创建对应目录
# src 必须是 dockerfile 所在路径的相对路径
```

### VOLUME

指定挂载持久化目录，创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。

```dockerfile
VOLUME ['/data', '/path/to/dir']
```

`VOLUME` 卷可以容器间共享和重用

但容器并不一定要和其他容器共享卷

修改卷后会立即生效

对卷的修改不会对镜像产生影响

卷会一直存在，知道没有任何容器在使用它



### CMD

构建容器后调用，即在容器**启动**时才会进行调用

而 RUN 是用于镜像**构建**时指定的命令，

```dockerfile
CMD ["executable","param1","param2"] 
# 执行可执行文件，优先选择
CMD ["param1","param2"] 
# 设置了ENTRYPOINT，则直接调用ENTRYPOINT并添加参数
CMD command param1 param2 
# 执行shell内部命令
```

每个dockerfile只能有一条`CMD`命令，如果指定多条，则只有最后一条会被执行。若用户在启动容器时指定了运行的命令，则会覆盖掉 CMD 指定的命令



### ENTRYPOINT

配置容器启动后执行的命令，并且不可被 docker run 提供的参数覆盖。每个 dockerfile 中只能有一个 `ENTRYPOINT`，当指定多个时，只有最后一个起效

配合CMD可省去"application"，只使用参数。

```dockerfile
ENTRYPOINT ["executable", "param1", "param2"] 
# 可执行文件, 优先)
ENTRYPOINT command param1 param2 
# shell内部命令
```

ENTRYPOINT 和 CMD 的区别：ENTRYPOINT 指定了该镜像启动时的入口，CMD 则指定了容器启动时的命令，当两者共用时，完整的启动命令像是 ENTRYPOINT + CMD 这样。使用 ENTRYPOINT 的好处是在我们启动镜像就像是启动了一个可执行程序，在 CMD 上仅需要指定参数；另外在我们需要自定义 CMD 时不容易出错。



### LABEL

用于为镜像添加元数据

```dockerfile
LABEL <key>=<value> <key>=<value> <key>=<value> ...
LABEL version="1.0" description="xxx"
```



### ARG

用于指定传递给构建运行时的变量

```dockerfile
ARG <name>[=<default value>]
# ARG 是唯一可放在 FROM 指令前的指令
```



### USER

指定运行容器时的用户名或 UID或 GID，也可两者组合，后续的 RUN 也会使用的指定用户，当服务不需要管理员权限时，可以通过该命令指定运行用户，并且可以在之前创建所需要的用户

通过 docker run 运行容器时，可以通过`-u`参数来覆盖所指定的用户

```dockerfile
USER user
USER user:group
USER uid:gid
```



### ONBUILD 

镜像触发器，在构建本镜像时不生效，在基于此镜像构建其他镜像时会触发

```dockerfile
ONBUILD [INSTRUCTION]
ONBUILD ADD . /app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
```



## 创建镜像

```bash
docker build -t <name> <dest>
-t 参数用来指定 image 文件的名字，后面可以用冒号指定标签，若不指定，则默认 latest
dest 表示打包路径   . 即表示当前文件所在路径
```



## 生成容器

```dockerfile
docker run -p <port>:<port> -it <name:tag> 
```



## 其他指令

**1.登陆/退出**

`docker [login/logout] [仓库地址]`



**2.查找镜像**

从 Docker Hub 搜索镜像

```bash
docker search [条件]
--automated=false   #  Only show automated builds
--no-trunc=false     # Don't truncate output
-s, --stars=0        # Only displays with at least xxx stars
```





**3.获取镜像**

除了官方仓库外的第三方仓库要指定 url，用户名就是在对应仓库下建立的账户，一般只有应用名的仓库代表官方镜像

```bash
docker pull [name]:[tag]
# 仓库格式为 [仓库url]/[用户名]/[应用名] 
```



**4.推送镜像**

```bash
docker push [image]:[tag]
```



**5.查看本地镜像**

`docker images`



**6.删除本地镜像**

`docker rmi [name | id]`

若是把 id 作为参数，可以只输入前几位，能唯一确定即可。也可以同时删除多个镜像，空格隔开



**7.查看镜像详情**

`docker inspect [name | id]`



**8.镜像tag**

`docker tag [name | id]  [username]/[repository]:[new tag]`



**9.创建、启动容器并执行命令**

```bash
docker run [params] [image_name | id] [command]
# 没有指定 command 时就执行镜像中默认的指令
# 常用命令
-d			后台运行容器并返回容器 id，不指定时启动后会开始打印日志，退出命令同时也会关闭容器

-i			以交互模式运行容器，通常与 -t 同时使用

-t			为容器重新分配一个未输入终端，通常 -it 来使用，表示容器的 shell 映射到当前的 shell，然后在本机窗口输入的命令，就会传入容器

--name alias 为容器指定一个别名

-h docker-name 设置容器的主机名，默认随机生成

--dns 8.8.8.8 	指定容器使用的 DNS 服务器，默认和宿主机一直

-e docker_host=0.0.0.0 设置环境变量

--expose port 开放一个端口或一组端口，会覆盖镜像设置中开放的端口

-p [宿主机端口]:[容器内端口]	 宿主机到容器的端口映射，

-P 	大写P，宿主机随机指定一组可用的端口映射容器 expose 的所有端口


--cpuset="0,1,2"	绑定容器到指定 CPU 运行
-m 100M						设置容器使用内存最大值
--net bridge			指定容器的网络连接类型，支持 bridge/host/none/container 四种类型
-v [宿主机目录路径]:[容器内目录路径] 挂载宿主机的指定目录到容器内的指定目录

```



**10.查看运行中容器**

`docker ps`



**11.开启/停止/重启容器**

```bash
docker stop CONTAINER	# 关闭容器
docker kill CONTAINER	# 强制关闭容器
docker start CONTAINER # 启动容器
docker restart CONTAINER		# 重启容器
docker pause CONTAINER	#暂停容器
docker create  # 创建容器，但不启动
```



**12.删除容器**

`docker rm [name | id]`

可以指定多个容器一起删除，加 `-f`可以强制删除正在运行的容器



**13.查看容器详情**

`docker inspect [name | id]`



**14.将容器保存为镜像**

`docker commit [name | id] [镜像名]:[tag]`

**15.打包本地镜像，使用压缩包完成迁移**

`docker save [image name] > [path]`

`docker save alpine > /usr/anyesu/docker/alpine.img`



**16.导入镜像压缩包**

`docker load < [path]`



## 四种网络模式

在`docker run`创建容器时可以通过`--net`指定网络模式

# 