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
	