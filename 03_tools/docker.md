### 查看容器挂载点

    sudo docker inspect a-team_wordpress | grep Mounts -A 20


### python容器

    FROM python 
    RUN pip install matplotlib

### 文件拷贝
注：可以双向拷贝，交换位置即可

    docker cp /opt/test container:/opt/test

### Dockerfile

compile it to images： 

    docker build ./ [-f Dockerfile] [-t nametag]

#### 多阶段构造
详情见[链接](https://docs.docker.com/develop/develop-images/multistage-build/)

两种方式，都挺方便

- Use an external image as a “stage”
- Use a previous stage as a new stage

### docker container

run -it and specify a volume, 原理是将当前tty attach到内部的shell的stdin/stdout, 参见[链接](https://docs.docker.com/engine/reference/run/): 

    docker run -it ltmapper /bin/bash

### GUI-application

Give docker the rights to access the X-Server with:

    xhost +local:docker

Set cmd correctly

    docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix image-name /bin/bash

### rm

rm all stopped containers

    docker container prune

rm all images with the name none

    docker images | grep none | awk '{ print $3;}' | xargs docker rmi


