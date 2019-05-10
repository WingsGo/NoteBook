    docker version	#docker版本号
	docker info		#docker信息
	docker run hello world	#测试docker是否安装成功

## 基本操作
	
	docker image ls 	#列出docker镜像
	docker image ls -a 	#列出所有docker镜像
	docker run ubuntu:16.04 echo "hello world"	#启动容器运行hello world
	docker run -p 8080:80 --name nginx_web -it nginx /bin/bash	#-i交互式 -t终端	-p映射端口
	
