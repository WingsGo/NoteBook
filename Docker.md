## Docker基本信息
    docker version	#docker版本号
	docker info		#docker信息
	docker run hello world	#测试docker是否安装成功

## Docker镜像操作
	docker pull [option] [Docker Registry 地址[:端口号]/]仓库名[:标签]
	docker run -it --rm ubuntu:16.04 bash	#--rm运行后删除
	docker system df
	docker image ls
	docker image ls -a
	docker image ls ubuntu
	docker image ls -f since/before=mongo:3.2
	docker image prune	#删除所有虚悬镜像
	docker image rm [选项] <镜像1> [<镜像2>]
	docker image rm $(docker image ls -q redis)		#删除所有名为redis的镜像
	docker image rm $(docker image ls -f before=mongo:3.2)	#删除所有mongo以后的镜像

	docker run --name webserver -d -p 80:80 nginx	#启动nginx容器
	docker exec -it webserver bash		#进入容器

	docker commit [选项] <容器ID或容器名> [<仓库名>[:<标签>]]
	docker commit --author "Tao Wang <twang2218@gmail.com>" --message "修改了默认网页" webserver nginx:v2

## Dockerfile
	---------------------------Simple Example-------------------------------
	FROM nginx
	RUN echo '<h1>Hello Docker</h1>' > /usr/share/nginx/html/index.html
	# 两种形式执行命令 (1)RUN <命令>  (2)RUN ["可执行程序", "参数1", "参数2"...]
	------------------------------------------------------------------------

	------------------------------------------------------------------------
	docker build -t nginx:v3 .	#根据Dockerfile定制镜像
	docker build [选项] <上下文路径/URL/->	#指定上下文路径，让服务器获得本地文件
	COPY ./package.json /app/	#复制上下文路径下的package.json,COPY等均使用相对路径

	# Docker build其他用法
	# 1. Git Repo或URL中创建
	docker build https://github.com/twang2218/gitlab-ce-zh.git#:8.14 
	# 2. 用压缩包创建
	docker build http://server/context.tar.gz
	# 3. 从stdin中读取
	docker build - < Dockerfile
	# 4. 从压缩包中读取
	dockr build - < context.tar.gz
	------------------------------------------------------------------------

	------------------------------------------------------------------------
	# 在 COPY 和 ADD 指令中选择的时候，可以遵循这样的原则，所有的文件复制均使用COPY 指令，仅在需要自动解压缩的场合使用 ADD
	COPY <源路径>... <目标路径>
	COPY ["<源路径1>",... "<目标路径>"]
	
	# CMD容器启动命令
	shell 格式： CMD <命令>	#CMD echo $HOME
	exec 格式： CMD ["可执行文件", "参数1", "参数2"...]	#CMD [ "sh", "-c", "echo $HOME" ]
	参数列表格式： CMD ["参数1", "参数2"...] 。在指定了 ENTRYPOINT 指令后，用 CMD 指定具体的参数。
	------------------------------------------------------------------------	

	------------------------------------------------------------------------
	# ENTRYPOIN指令
	# 1.让镜像变成命令一样使用，可以添加参数
	CMD [ "curl", "-s", "http://ip.cn" ]
	docker run myip -i 
	# 此时用-i覆盖了默认的curl -s http://ip.cn，正确写法为 docker run myip curl -s http://ip.cn -i
	
	# 正确写法为
	ENTRYPOINT [ "curl", "-s", "http://ip.cn" ]
	docker run myip -i	# CMD内容会作为参数传递给entrypoint,此时-i为新的CMD

	# 2.应用运行前的准备工作
	ENTRYPOINT ["docker-entrypoint.sh"]		#开启服务前运行脚本
	CMD ["redis-server"]		#默认以redis-server身份运行
	docker run -it redis id		#切换到redis身份运行
	------------------------------------------------------------------------

	------------------------------------------------------------------------
	# ENV设置环境变量
	ENV <key> <value>
	ENV <key1>=<value1> <key2>=<value2>...
	ENV NODE_VERSION 7.2.0 or ENV NODE_VERSION=7.2.0
	------------------------------------------------------------------------

	------------------------------------------------------------------------
	# EXPOSE 声明端口
	EXPOSE <端口1> [<端口2>...]	#声明运行时容器提供服务端口,在运行时并不会因为这个声明应用就会开启这个端口的服务
	------------------------------------------------------------------------

	------------------------------------------------------------------------
	# WORKDIR 指定工作目录
	WORKDIR <工作目录路径>	#各层的当前目录就被改为指定的目录，如该目录不存在， WORKDIR 会帮你建立目录。
	------------------------------------------------------------------------

	------------------------------------------------------------------------
	# USER 指定当前用户
	USER <用户名>

	# 使用 gosu在执行期间改变身份
	RUN groupadd -r redis && useradd -r -g redis redis
	# 下载 gosu
	RUN wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.7/
	gosu-amd64" \
	&& chmod +x /usr/local/bin/gosu \
	&& gosu nobody true
	# 设置 CMD，并以另外的用户执行
	CMD [ "exec", "gosu", "redis", "redis-server" ]
	------------------------------------------------------------------------