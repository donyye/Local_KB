K8s-Namespace-cgroup

- Namespace 和 cgroup

   

  容器技术发展历史：

  ![[My-Case_runing_008_K8s-Namespace-cgroup_001.png]]

   

   

  Docker 容器实现原理

  Docker 容器在实现上通过 namespace 技术实现进程隔离。

  通过cgroup 技术实现容器进程可用资源的限制

  ![[My-Case_runing_008_K8s-Namespace-cgroup_002.png]]

   

  VM 是通过虚拟硬件来实现隔离

  Namespace 命名空间：

  容器是通过进程，每个容器隔离是通过 namespace ，相当于每个进程都在一个盒子里面。主要是为了资源隔离。

  Namespace 将kernel的全局资源进行封装，使得每个 Namespace 都有一份独立的资源。因此不同进程在各自Namespace内对同一个钟资源的使用不会相互干扰。

   

  - PID Namespace 隔离示例

  <!-- -->

  - 以交互式启动一个CentOS容器，并在其中运行 /bin/bash 程序。执行ps命令查看到 "/bin/bash"是 PID=1 的进程，Docker将其隔离于宿主机中的其他进程

   

  # docker run -itd centos:latest /bin/bash

  c61098c403438f32635d6b2cc5bd909cc288ba346412f62951aee766fd9f47f1

   

  # docker ps

  CONTAINER ID   IMAGE           COMMAND       CREATED         STATUS         PORTS     NAMES

  c61098c40343   centos:latest   "/bin/bash"   4 seconds ago   Up 3 seconds             vigilant_gagarin

   

  - 在容器里看到的进程ID是1

  # docker exec -it vigilant_gagarin ps axf

      PID TTY      STAT   TIME COMMAND

       21 pts/1    Rs+    0:00 ps axf

       [ 1 pts/0    Ss+    0:00 /bin/bash]

   

  - 实际宿主机里看到这个容器的 PID 进程号是 161723

  # docker inspect vigilant_gagarin |grep -i pid

              "Pid": 161723,

              "PidMode": "",

              "PidsLimit": null,

   

  - 所以你可以过滤这个PID,看到这个进程，也进一步说明的容器就是一个进程。

  # ps aux |grep 161723

  root      161723  0.0  0.0  12052  3272 pts/0    Ss+  15:30   0:00 /bin/bash

  root      161996  0.0  0.0  12116   720 pts/0    S+   15:38   0:00 grep --color=auto 161723

   

  分别在宿主机和容器中查看该容器进程相关的namespace信息，发现两者是一致的

  ![[My-Case_runing_008_K8s-Namespace-cgroup_003.png]]

  每个进程在 /proc 下都有一个目录，存放 namespace 相关信息。

   

  - Cgroups : Linux Control Group

  <!-- -->

  - 作用：限制一个进程组队系统资源的使用上限，包括cpu、内存、Block I/O 等

  Cgroups 还能设置进程优先级，对进程进行挂起和恢复等操作。

  - 原理：将一组进程放在一个Cgroup中，通过给这个Cgroup分配指定的可用资源，达到控制这一组进程可用资源的目的。
  - 实现：在 Linux 中，Cgroups 以文件和目录的方式组织在操作系统的 /sys/fs/cgroup 路径下。

  该路径中所有的资源种类均可被 cgroup 限制

   

  # mount -t cgroup

  cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,name=systemd)

  cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpu,cpuacct)

  cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)

  cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)

  cgroup on /sys/fs/cgroup/rdma type cgroup (rw,nosuid,nodev,noexec,relatime,rdma)

  cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)

  cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)

  cgroup on /sys/fs/cgroup/net_cls,net_prio type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls,net_prio)

  cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,pids)

  cgroup on /sys/fs/cgroup/misc type cgroup (rw,nosuid,nodev,noexec,relatime,misc)

  cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)

  cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)

  cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
