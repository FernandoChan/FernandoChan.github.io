---
layout: post
title: "Linux shell 查看文件后缀名的三种方式"
subtitle: "imgur+fu"
author: "Fernando"
header-img: "img/wsl.jpg"
header-mask: 0.3
catalog: true
tags:
  - linux shell
---


## Linux shell 查看文件后缀名的三种方式

1. `ls -l` or `ll`  可以列出当前目录下面的所有文件的详细信息, 也包括了一些隐藏文件, 就像这样

```shell
fernando@WIN10-608190930:~$ ll
total 32
drwxr-xr-x 1 fernando fernando   512 Nov 12 15:35 ./
drwxr-xr-x 1 root     root       512 Nov 10 09:41 ../
-rw------- 1 fernando fernando  3340 Nov 15 21:06 .bash_history
-rw-r--r-- 1 fernando fernando   220 Nov 10 09:41 .bash_logout
-rw-r--r-- 1 fernando fernando  3861 Nov 10 10:45 .bashrc
drwxrwxrwx 1 fernando fernando   512 Nov 10 18:36 .bundle/
drwxrwxrwx 1 fernando fernando   512 Nov 10 10:45 .gem/
-rw------- 1 fernando fernando    40 Nov 12 13:28 .git-credentials
-rw-rw-rw- 1 fernando fernando   110 Nov 10 14:19 .gitconfig
-rw-r--r-- 1 fernando fernando   807 Nov 10 09:41 .profile
-rw-r--r-- 1 fernando fernando     0 Nov 10 09:44 .sudo_as_admin_successful
-rw------- 1 fernando fernando 18830 Nov 12 15:35 .viminfo
drwxrwxrwx 1 fernando fernando   512 Nov 12 13:25 FernandoChan.github.io/
drwxrwxrwx 1 fernando fernando   512 Nov 12 15:35 c_files/
drwxrwxr-x 1 fernando fernando   512 Nov 10 18:38 gems/
drwxrwxrwx 1 fernando fernando   512 Nov 10 18:36 test-site/
fernando@WIN10-608190930:~$ ls
FernandoChan.github.io  c_files  gems  test-site
```

只有最后面四个是真正的文件

2. `file filename`  相对清爽很多
3. `stat filename`  列出了这个文件的所有详细内容, 包括所在的物理内存和修改时间等信息
```shell
fernando@WIN10-608190930:~/FernandoChan.github.io/img$ stat post-bg-web.jpg
  File: post-bg-web.jpg
  Size: 116226          Blocks: 232        IO Block: 512    regular file
Device: 2h/2d   Inode: 3377699721160804  Links: 1
Access: (0666/-rw-rw-rw-)  Uid: ( 1000/fernando)   Gid: ( 1000/fernando)
Access: 2018-11-10 11:02:12.796528400 +0800
Modify: 2018-11-10 11:02:12.800510300 +0800
Change: 2018-11-10 11:02:12.800510300 +0800
 Birth: -
```



综合起来对比一下


```sh
fernando@WIN10-608190930:/home$ ls
fernando
fernando@WIN10-608190930:/home$ ls -l
total 0
drwxr-xr-x 1 fernando fernando 512 Nov 12 15:35 fernando
fernando@WIN10-608190930:/home$ ll
total 0
drwxr-xr-x 1 root     root     512 Nov 10 09:41 ./
drwxr-xr-x 1 root     root     512 Nov 10 09:39 ../
drwxr-xr-x 1 fernando fernando 512 Nov 12 15:35 fernando/
fernando@WIN10-608190930:/home$ stat fernando/
  File: fernando/
  Size: 512             Blocks: 0          IO Block: 512    directory
Device: 2h/2d   Inode: 1125899907261744  Links: 1
Access: (0755/drwxr-xr-x)  Uid: ( 1000/fernando)   Gid: ( 1000/fernando)
Access: 2018-11-10 09:41:11.451726600 +0800
Modify: 2018-11-12 15:35:24.619811700 +0800
Change: 2018-11-12 15:35:24.619811700 +0800
 Birth: -
fernando@WIN10-608190930:/home$ cd fernando/
fernando@WIN10-608190930:~$ ls
FernandoChan.github.io  c_files  gems  test-site
fernando@WIN10-608190930:~$ file gems
gems: directory
```

