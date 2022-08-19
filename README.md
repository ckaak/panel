安装docker和python环境：

```
apt-get install docker.io -y
```

```
cat <<"EOF" | bash                              
sudo apt update && \
sudo apt install software-properties-common && \
sudo add-apt-repository ppa:deadsnakes/ppa && \
sudo apt install python3.9 -y && \
python3.9 --version
EOF
```

docker创建网络：

```
docker network create cdntip_network
```

启动mysql容器：


```
mkdir /data
docker run -d -it --network cdntip_network -v /data/mysql:/var/lib/mysql --name panel_mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=panel mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

启动 cloudpanel：

```
docker run -d -it --network cdntip_network -p 8111:80 --name panel cdntip/panel
```

进入容器：

```
docker exec -it panel /bin/bash
```
创建管理员：

```
python manage.py createsuperuser --username admin --email admin@admin.com
```

添加aws镜像：

```
python manage.py aws_update_images
```
注：请按CTRL+D退出当前终端

打开浏览器，输入  ip:8111


设置开机启动以及容器守护进程：

```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

输入docker ps -a，查看容器ID（CONTAINER ID），然后docker update --restart=always xxxxxxxx（xxxxxx为CONTAINER ID 容器ID）




其它说明
- 后端暂时未上传到github, 但是代码都是未加密的, 在容器中可以看到。
- docker 暂时只有x86平台(不支持arm平台)
- 目前版本为预览版，有问题请到群里反馈 @cdntip
