# docker 笔记

## Docker 基本指令

### Container

1. docker run
    - -p {host_port}:{container_port}
    - -v {host_path}:{container_path}
    - -d 后台执行
    - --name 容器名
    - --link <name or id>:alias /alias是源容器在link 下的别名
    - --help 查看其它参数咯，还有很多

2. docker stop

3. docker rm 

4. docker kill
    > stop是向容器发送一个终止信号， 进行优雅关闭， kill 发送了 kill -9信号， 强行关闭

5. exec  [OPTIONS] CONTAINER COMMAND [ARG...]
    - -it  与tty交互

### Image

1. pull 
2. ls
3. rmi

### extra
1. docker system df 查看空间占用
2. docker image prune
3. docker container prune
4. docker ps -aq 列出所有容器 id
5. docker stop $(docker ps -aq)
6. docker rmi $(docker images -q)
7. docker system prune

## Dockerfile 指令
1. docker build -t [image]:[tag] . 
    > . 代表上下文目录

2. FROM [imagesName:tag] 
    - 默认 tag 为 latest
3. RUN
    > 每一个 run 都会建立新的一层， 因此每一行结尾可以加一个 \ ，减少层数

    - shell 格式
        > RUN <命令> 直接执行 shell 命令
    - exec 格式
        > RUN ["可执行文件","参数1","参数2"]
3. COPY <原路径> ... <目标路径>
4. ADD 和 COPY基本一致， 但是能够发起网络请求
5. CMD 容器启动命令
    - cmd 用于指定容器主进程的启动命令
    - CMD ["参数1","参数2","参数3"]
    - 相当于 docker run -it [/bin/bash] bash 就是启动命令
6. ENTRYPONIT 入口点
    - 和 run 命令保持一致， 相当于在命令加前缀
7. ENV key=value
9. ARG 与 env 效果一致，代表构建时的环境变量
10. VOLUME 匿名挂载卷
    - 对于需要写操作的容器来说，提供一个匿名的挂载卷，防止应用重启后，文件丢失
    - 可被覆盖
    - <主机path>:<容器path>
11. EXPOSE 暴露端口
12. WORKDIR 指定工作目录
    - 如果目录不存在，会自动创建
13. USER username
    - 必须事先创建好 user
14. HEALTHCHECK 健康检查 不健康的容器不会被调度
    - --interval
    - --timeout
    - --retries
15. ONBUILD 下层镜像构建时使用

## 数据卷 volumes

数据卷是一个可以在一个或多个容器共享使用的目录

- 容器间共享和使用
- 对数据卷修改会立马生效
- 对数据卷更新不影响镜像
- 数据卷默认一直存在， 即使删除容器
- --mount source=<volume,path> ,target=container path

> 数据卷使用，类似 linux 下的挂载

## network

网络可以使多个容器能够通信，docker提供了两种网络类型 `bride` ,`overlay`

1. docker network create -d bridge zdpdpdp_network
2. docker run -it --rm --name b1 --network zdpdp busybox sh
3. docker run -it --rm --name b2 --network zdpdp busybox sh

进入b1后， 可以 ping 通b2



