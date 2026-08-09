K8s-volume-4.1

-  

  ![[My-Case_runing_006_K8s-volume-4.1_001.png]]

   

   

  - Bind mount 本地存储到容器里

  # docker run -d -p 8081:80 -v /root/htdocs:/usr/local/apache2/htdocs/ httpd

  576d3e704caa765e8a748aac4651d33866df89b3951677278a9cd0a3397504c3

  # 前面" /root/htdocs"是本地的存储， 如果没有会自动创建

   

  - 创建一个 html 然后测试

  # echo haha \> /root/htdocs/index.html

   

  # curl <http://localhost:8081>

  haha

   

  - 查看 httpd 容器已经在运行

  # docker ps

  CONTAINER ID   IMAGE      COMMAND                  CREATED              STATUS              PORTS                                         NAMES

  576d3e704caa   httpd      "httpd-foreground"       About a minute ago   Up About a minute   0.0.0.0:8081-\>80/tcp, [::]:8081-\>80/tcp       quirky_goldberg

  8d295fc2f8ab   httpd      "httpd-foreground"       About an hour ago    Up About an hour    80/tcp                                        mystifying_margulis

   

  - 进入到容器里，查看html是否存在，可以看到时在的。

  # docker exec -it quirky_goldberg /bin/bash

  root@576d3e704caa:/usr/local/apache2# cat /usr/local/apache2/htdocs/index.html

  Haha

   

  - 先停止容器后再删除容器，看数据是否还在

  # docker stop quirky_goldberg

  quirky_goldberg

   

  # docker rm quirky_goldberg

  quirky_goldberg

   

  - 可以看到容器被删除后数据还是在的

  # cat /root/htdocs/index.html

  Haha
