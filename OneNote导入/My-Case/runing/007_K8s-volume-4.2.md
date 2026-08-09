K8s-volume-4.2

-  

  ![[My-Case_runing_007_K8s-volume-4.2_001.png]]

   

   

   

   

  ![[My-Case_runing_007_K8s-volume-4.2_002.png]]

   

   

  数据共享可以主机和容器，也可以容器与容器共享

   

   

  - 通过 bind mount 实现容器间数据共享

   

  - 创建两个容器，h1和h2，挂载volume时候使用的相同的地址，不同的端口。

  # docker run -d --name h1 -p 1001:80 -v /root/htdocs:/usr/local/apache2/htdocs httpd

  24210efc309bada50ddbcd4d9191be6ff89b351f469a4267cec37a79cd236a3c

   

  # docker run -d --name h2 -p 1002:80 -v /root/htdocs:/usr/local/apache2/htdocs httpd

  b5769e3ae05eb5a534843b8a78549bc842d67bbd427c9fd50733b3805bc89efd

   

  # cat /root/htdocs/index.html

  haha

   

  - 从测试可以看到，访问都是相同volume的数据。

  # curl localhost:1001

  haha

  # curl localhost:1002

  haha

   

  - 修改html后看到的还是相同修改后的信息。

  root@k81node:~# echo hehe \> /root/htdocs/index.html

   

  root@k81node:~# curl localhost:1001

  hehe

  root@k81node:~# curl localhost:1002

  hehe

   

   

  - Volume container 实现容器间的数据共享

   

  - 先创建一个容器，挂载volume

  # docker run -d --name vc -v /root/htdocs:/usr/local/apache2/htdocs httpd

  eb99d8a61635525e7cfca37c10477deeeed75bd890d07fcaa6d2c0041884efea

   

  - 然后再创建 h3 和 h4，通过" --volumes-from"参数，我的卷从那里获取，这里是VC。

  # docker run -d --name h3 -p 1003:80 --volumes-from vc httpd

  70ab61c84d9026df2bd9648c872d9fcc129eb997c7b71bc0683a8113e2d85ad7

   

  # docker run -d --name h4 -p 1004:80 --volumes-from vc httpd

  7582077348b71fce8183682bd9e4a667cda896abb6e0a4a9f4e35a7cac73b6a5

   

  - 查看 vc 、h3、h4 容器的挂载的volume都是同一个

  # docker inspect vc |grep -A 4 'Mounts'

          "Mounts": \

  [            {

                  "Type": "bind",

                  "Source": "/root/htdocs",

                  "Destination": "/usr/local/apache2/htdocs",

   

  # docker inspect h3 |grep -A 4 'Mounts'

          "Mounts": \

  [            {

                  "Type": "bind",

                  "Source": "/root/htdocs",

                  "Destination": "/usr/local/apache2/htdocs",

   

  # docker inspect h4 |grep -A 4 'Mounts'

          "Mounts": \

  [            {

                  "Type": "bind",

                  "Source": "/root/htdocs",

                  "Destination": "/usr/local/apache2/htdocs",

   

  - 修改html然后测试h3和4，看到的结果是一样的。

  root@k81node:~# echo new \> /root/htdocs/index.html

   

  root@k81node:~# curl [http://localhost:1003](http://localhost:1003)

  new

  root@k81node:~# curl [http://localhost:1004](http://localhost:1004)

  new
