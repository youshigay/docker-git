docker 私人仓库
# 获取registry镜像
docker pull registry
# 生成镜像容器
# /Users/youzhigang228/registry为保存私人仓库的地址
# /tmp/registry 为容器目录
docker run -d -p 5000:5000 -v /Users/youzhigang228/registry:/tmp/registry registry
(之后再docker push 中会有问题，到时候再看)
使用docker ps可以到registry的容器已经有了
# 镜像打ag
# mydockerapp：1.4 为我们现在的镜像
# 10.180.170.45/mydockerapp为私人仓库路径
docker tag mydockerapp:1.4 10.180.170.45:5000/mydockerapp
# 把镜像push到私人仓库当中
docker push 10.180.170.45:5000/mydockerapp
此步骤会出现问题：
  Get https://10.180.170.45:5000/v2/: http: server gave HTTP response to HTTPS client
修改OS x中的docker preferences 中的Insecure registries中加入10.180.170.45:5000;
接下来出现一直等待，received unexpected HTTP status: 503 Service Unavailable
解决：
  重新删掉registry容器，加入--privileged=true

启动registry容器为：
docker run -d -p 5000:5000 -v /Users/youzhigang228/registry:/tmp/registry --privileged=true  registry

docker pull 成功

# 删除本地镜像
docker rmi -f 10.180.170.45:5000/mydockerapp 
# 查看本地镜像
docker images
# 从私人仓库中拉取镜像
docker pull 10.180.170.45:5000/mydockerapp
# 查看是否已经拉取
docker images

# 查看私人仓库镜像
curl -XGET http://10.180.170.45:5000/v2/_catalog
# 查看私人仓库某个镜像的标签列表
curl -XGET http://10.180.170.45:5000/v2/mydockerapp/tags/list