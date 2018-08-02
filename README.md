docker pull node
cd 到具体的目录
完成package.json和server.js
新建Dockerfile
docker build -t mydockerapp:1.0 . #生成相应镜像，使用docker images可以查看
docker run -d -p 9091:9091 mydockerapp:1.0 #前面代表自己的端口，后面代表docker的端口


Dockerfile 中
run 的命令是在docker build的镜像执行的
cmd的命令是在docker run 镜像生成容器的时候执行的
比如npm install 是在docker build生成镜像的时候完成，如果之后npm install，镜像中没a有相关模块，是跑不起来的

如果在生成镜像之后rm -rf node_modules, 对docker run是没有影响的


生成部署，可以使用打包镜像，然后上传到生成，然后loader解压镜像，然后docker run

#docker save <namespace>/<name:tag> <name>.tar
例子：docker save lzqs/deploy:1.0 > deploy.tar

在生产环境：docker load < deploy.tar 生成镜像
使用docker images查看