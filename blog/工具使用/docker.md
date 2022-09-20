stop所有镜像：  docker stop $(docker ps -a -q)
删除container： docker container prune
删除镜像：docker system prune -a

删除untagged images：docker rmi $(docker images | grep “^” | awk “{print $3}”)
删除所有镜像：docker rmi $(docker images -q)


(0571)8289-1152
0571 8285-1141
8353 3095

