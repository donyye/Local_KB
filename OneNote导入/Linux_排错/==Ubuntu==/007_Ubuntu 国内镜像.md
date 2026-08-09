Ubuntu 国内镜像

2025年7月30日

9:14

\# 注意，不同版本的原配置地方有所改变\
\
Ubuntu 24.04[  ]

/etc/apt/sources.list.d/ubuntu.sources

 

不用所有的加，加其中一两个就行，要不是update的时间会很长。

 

\# 阿里云

Enabled: yes

Types: deb

URIs: <http://mirrors.aliyun.com/ubuntu/>

Suites: noble noble-updates noble-security

Components: main restricted universe multiverse

Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

 

\# 清华源

Enabled: yes

Types: deb

URIs: <http://mirrors.tuna.tsinghua.edu.cn/ubuntu/>

Suites: noble noble-updates noble-security

Components: main restricted universe multiverse

Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

 

\# 中科大源

Enabled: yes

Types: deb

URIs: <http://mirrors.ustc.edu.cn/ubuntu/>

Suites: noble noble-updates noble-security

Components: main restricted universe multiverse

Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

 

\# 网易源

Enabled: yes

Types: deb

URIs: <http://mirrors.163.com/ubuntu/>

Suites: noble noble-updates noble-security

Components: main restricted universe multiverse

Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

 

\# 腾讯源

Enabled: yes

Types: deb

URIs: <http://mirrors.tencent.com/ubuntu/>

Suites: noble noble-updates noble-security

Components: main restricted universe multiverse

Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

\"/etc/apt/sources.list.d/ubuntu.sources\" 53L, 1549B

 

Enabled: no[   \<\-- ]不使用的不激活

Types: deb

URIs: <http://archive.ubuntu.com/ubuntu/>

Suites: noble noble-updates noble-backports

Components: main restricted universe multiverse

Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

 

Enabled: no[  \<\-- ]不使用的不激活

Types: deb

URIs: <http://security.ubuntu.com/ubuntu/>

Suites: noble-security

Components: main restricted universe multiverse

Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

 

 

 

Ubuntu 22.04

/etc/apt/sources.list

deb <https://mirrors.aliyun.com/ubuntu/> noble main restricted universe multiverse

deb-src <https://mirrors.aliyun.com/ubuntu/> noble main restricted universe multiverse

 

deb <https://mirrors.aliyun.com/ubuntu/> noble-security main restricted universe multiverse

deb-src <https://mirrors.aliyun.com/ubuntu/> noble-security main restricted universe multiverse

 

deb <https://mirrors.aliyun.com/ubuntu/> noble-updates main restricted universe multiverse

deb-src <https://mirrors.aliyun.com/ubuntu/> noble-updates main restricted universe multiverse

 

\# deb <https://mirrors.aliyun.com/ubuntu/> noble-proposed main restricted universe multiverse

\# deb-src <https://mirrors.aliyun.com/ubuntu/> noble-proposed main restricted universe multiverse

 

deb <https://mirrors.aliyun.com/ubuntu/> noble-backports main restricted universe multiverse

deb-src <https://mirrors.aliyun.com/ubuntu/> noble-backports main restricted universe multiverse

 

 

更新：

 

sudo apt-get update

 

sudo apt-get upgrade

 

 

 

 

 

 

 

 

已使用 OneNote 创建。
