K8s-volume-4

- 容器的 volume

   

  - 查看volume是没有的，因为没运行容器

  # docker volume ls

  DRIVER    VOLUME NAME

   

  - 运行一个 httpd 容器

  # docker run -d httpd

  8d295fc2f8ab5e87ea9f13f037e815e108c90af7a229d692455e87c7fa0320e1

   

  - 查看运行的容器

  # docker ps

  CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                                         NAMES

  8d295fc2f8ab   httpd      "httpd-foreground"       8 seconds ago   Up 7 seconds   80/tcp                                        mystifying_margulis

   

  - 进入到容器查看 httpd 存放 html 的位置

  # docker exec -it mystifying_margulis /bin/bash

  root@8d295fc2f8ab:/usr/local/apache2#

  root@8d295fc2f8ab:/usr/local/apache2# grep ^DocumentRoot /usr/local/apache2/conf/httpd.conf

  DocumentRoot "/usr/local/apache2/htdocs"

   

  - 重新运行一个容器，-v 是带 volume，指向容器存放 html的位置

  # docker run -d -p 8080:80 -v /usr/local/apache2/htdocs/ httpd

  2b666c179ee3d87e62c3622ccaa0add31eb9792366b8b0709bb2d715430a353f

   

  - 在查看一下容器是否被运行

  # docker ps

  CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                                         NAMES

  2b666c179ee3   httpd      "httpd-foreground"       6 seconds ago   Up 6 seconds   0.0.0.0:8080-\>80/tcp, [::]:8080-\>80/tcp       recursing_dijkstra

  8d295fc2f8ab   httpd      "httpd-foreground"       4 minutes ago   Up 4 minutes   80/tcp                                        mystifying_margulis

   

  - 现在可以看到有volume信息

  # docker volume ls

  DRIVER    VOLUME NAME

  local     78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c

   

  - 可以查看volume映射到本地什么地方 

  # docker volume inspect 78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c 

  \

  [    {

          "CreatedAt": "2025-05-15T09:20:05+08:00",

          "Driver": "local",

          "Labels": ,

          "Mountpoint": "/var/lib/docker/volumes/78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c/\_data",

          "Name": "78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c",

          "Options": null,

          "Scope": "local"

      }

  ]

   

  - 可以看到在volume映射到本地的位置

  # ls /var/lib/docker/volumes/78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c/

  \_data

   

  - 查看容器的volume信息

  # docker inspect recursing_dijkstra

  ......Output omitted......

          "Mounts": \

  [            {

             [     "Type": "volume",]

                 [ "Name": "78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c",]

                  "Source": "/var/lib/docker/volumes/78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c/\_data",

           [       "Destination": "/usr/local/apache2/htdocs",]

                  "Driver": "local",

  ......Output omitted......

   

  - 因为这个目录地址和本地是映射的，所以这里修改就是修改容器的目录。

  # cat /var/lib/docker/volumes/78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c/\_data/index.html 

  \<html\>\<body\>\<h1\>It works!\</h1\>\</body\>\</html\>

   

  # curl <http://localhost:8080>

  \<html\>\<body\>\<h1\>It works!\</h1\>\</body\>\</html\>

   

  # echo haha \> /var/lib/docker/volumes/78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c/\_data/index.html

   

  # curl <http://localhost:8080>

  haha

   

  - 强制删除正在运行的容器，然后检测数据是还在

  # docker rm recursing_dijkstra --force

  recursing_dijkstra

   

  - 删除容器后卷任然存在

  # docker volume ls

  DRIVER    VOLUME NAME

  local     78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c

   

  - 可以看到数据也在

  # cat /var/lib/docker/volumes/78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c/\_data/index.html 

  haha

   

  - 可以手动删除卷，数据也会被删除

  # docker volume rm 78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c 

  78300f17acb491c3944a43e1cfd0923b17f4912c08e242a7e1bb997c9e1b7a1c

   

  # docker volume ls

  DRIVER    VOLUME NAME
