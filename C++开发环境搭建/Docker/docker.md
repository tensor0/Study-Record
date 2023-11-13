### ubuntu下docker相关命令
1. 列出镜像：
    ```
    sudo docker images
    ```
2. 创建并运行一个容器
    ```shell
    docker run 
    -d //让容器在后台运行
    --name mysql //给容器起个名字，必须唯一
    -p 3306:3306 //设置宿主机端口到容器端口的映射：因为容器是隔离的，所以要把容器内镜像的端口映射到宿主机的端口 宿主机端口：容器内端口
    -e Tz=Asia/Shanghai //e:environment 是设置环境变量：-e KEY=VALUE
    -e MYSQL_ROOT_PASSWORD=123
    mysql //指定运行的镜像的名字

    镜像命名规范：
    镜像名称一般由两部分组成：[repository]:[tag]
    其中repository就是镜像名
    tag是镜像的版本
    在没有指定tag时，默认是latest(最新版本)，代表最新版本的镜像
    mysql：5.7//指的是5.7版本的mysql
    ```
### #docker常见命令
https://www.bilibili.com/video/BV1HP4118797?p=5&vd_source=781da99515e0c7473251cac85d8b8fb6