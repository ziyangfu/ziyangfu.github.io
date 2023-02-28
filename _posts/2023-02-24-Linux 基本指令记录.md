## Linux  指令大全

### 01.基础

```C
// 转换编码
iconv test.log -f GB2312 -t UTF-8 -o test1.log --verbose
grep -nr "onfiguration module could not be loaded!" // 报该项错误时，源代码中的相关文件与行号
sudo apt-cache search xxx // 查找软件源中的软件
    
dpkg -l | grep "asdad"  //  查找对应软件安装路径
    
tar -zxvf ./xxx.tar.gz//解压 tar.gz
tar -jzvf ./xxx.tar.bz2 // 解压 tar.bz2
unrar x ./xxx.rar  //  解压rar文件
// 强制删除正在运行的程序  
top | grep "xxx"  // 找出正在运行的程序
/*
怎么卸载通过 make install 安装的文件：
一般来说，make install完了目录下会有一个install_mainfest.txt的文件记录了安装的所有内容，然后xargs rm < install_manifest.txt就可以了。如果没有这个文件，可以自己重新make install，从log中过滤出install信息了。
*/
sudo xargs rm < install_manifest.txt
    
/*批量删除某个格式的文件*/
find ./ -name *.arxml | xargs rm -r
    
sudo gedit ~/.bashrc  // 打开环境变量
    
file xxx  // 查看可执行文件的架构，如arm x86_64等
//若是静态库，则使用以下指令
//查看目标文件或者可执行的目标文件的构成.
// -a 显示档案库的成员信息,类似ls -l将lib*.a的信息列出
objdump -f xx.a
    
/*
ldconfig命令的用途主要是在默认搜寻目录/lib和/usr/lib以及动态库配置文件/etc/ld.so.conf内所列的目录下，搜索出可共享的动态链接库（格式如lib*.so*）,进而创建出动态装入程序(ld.so)所需的连接和缓存文件。缓存文件默认为/etc/ld.so.cache
ldconfig通常在系统启动时运行，而当用户安装了一个新的动态链接库时，就需要手工运行这个命令
<若碰到那种库没法加载的问题，可以尝试运行这个指令，看看能不能解决>
*/   
ldconfig
    
//查看当前系统磁盘使用空间
df -h
//查看当前目录文件占用空间大小
du -sh *
    
// 退出 ssh登录
exit
    
// 添加环境变量的三种方式
1. export  // 临时用
2. ~/.bashrc  // 永久，当前用户
3. /bash.bashrc // 永久，全部用户
    
// 查看可执行文件链接的动态链接库
ldd <可执行文件>
    
// 手动挂载mmcblk0p3分区到mnt
// 这个首先通过 mount 指令查看当前挂载情况
mount /dev/mmcblk0p3 /mnt
// 列出加载在内核中的所有驱动   
dmesg
    //列出所有被检测到的硬件
	dmesg | grep sda
  	// 输出dmesg命令最后20行日志
    dmesg | tail -20
// 杀死进程
    ps -aux | grep "XXX"
sudo kill -s 9 <PID>
/*
linux进程状态
 D (TASK_UNINTERRUPTIBLE) 	不可中断的睡眠状态
               R (TASK_RUNNING)				正在运行，或在队列中的进程
               S (TASK_INTERRUPTIBLE)		可中断的睡眠状态
               T (TASK_STOPPED)				停止状态
               t (TASK_TRACED)				被跟踪状态
               Z (TASK_DEAD - EXIT_ZOMBIE)  退出状态，但没被父进程收尸，成为僵尸状态
               W    						进入内存交换（从内核2.6开始无效）
               X (TASK_DEAD - EXIT_DEAD)    退出状态，进程即将被销毁


               <    高优先级
               N    低优先级
               L    有些页被锁进内存
               s    包含子进程
               +    位于后台的进程组；
               l    多线程，克隆线程  multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
*/
// 远程拷贝
// scp ：security copy， 基于 ssh 协议
// 本机拷贝到远程
scp <local_dir> root@192.168.0.232:/home/root/dir
// 远程到本地 -r 拷贝文件夹
scp -r root@192.168.0.232:/home/root/dir <local_dir>
    
// env  添加环境变量
export PATH=$PATH:<your path>  // 增量添加
// 创建软链接
sudo ln -s ./home/xxx /usr/lib
// 创建硬链接
sudo ln ./home/xxx /usr/lib
    
// 图像压缩 https://blog.csdn.net/weixin_33774308/article/details/92345188
// https://www.jianshu.com/p/db7a88062f66
//jpg 用 jpegoptim，png 用 optipng
jpegoptim --dest=./ -S700 ../XXX.jpg 
//------------- VPN 相关 ------------------------------- //
// clash 关闭后，无法上网，查看原因可能是 DNS 修改
sudo systemctl start systemd-resolved
// 打开 clash 后，显示 DNS
//ubuntu20.04 :Start DNS server error: listen udp 0.0.0.0:53: bind: address already in use
// 原因是 DNS 端口占用
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
//------------- VPN 相关 ------------------------------- //
```

-----

###   02.网络

```bash
#// DNS
nslookup www.baidu.com // 查找 IP 地址与主机的对应关系
/*
对应的输出：
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
www.baidu.com	canonical name = www.a.shifen.com.
Name:	www.a.shifen.com
Address: 112.80.248.75    直接输入该 IP 地址，也可以直接访问百度主页
Name:	www.a.shifen.com
Address: 112.80.248.76
*/
ifconfig
#// 临时修改 IP 地址
ifconfig <网卡地址> 192.168.100.100
    
// ssh error
/*
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that the RSA host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
08:98:a9:cc:f8:37:20:6b:b4:b1:6c:3a:15:b9:a9:92.
Please contact your system administrator.
Add correct host key in /home/arnold/.ssh/known_hosts to get rid of this message.
Offending key in /home/arnold/.ssh/known_hosts:2
RSA host key for 10.18.46.111 has changed and you have requested strict checking.
Host key verification failed.*/
#// 解决办法：删除~/.ssh/known_hosts文件，然后重新连接
sudo rm ~/.ssh/known_hosts
    
    
ping&arping
#// 注意设置端口号
    
#//查看端口是否开放，没有显示，则没有开放
netstat -aptn | grep -i <端口号>
    
    
/**
https://www.cnblogs.com/little-monica/p/11459772.html
linux下使用tc和netem模拟网络异常（一）
Netem 是 Linux 2.6 及以上内核版本提供的一个网络模拟功能模块。该功能模块可以用来在性能良好的局域网中，模拟出复杂的互联网传输性能。例如:低带宽、传输延迟、丢包等等情况。使用 Linux 2.6 (或以上) 版本内核的很多 Linux 发行版都默认开启了该内核模块，比如：Fedora、Ubuntu、Redhat、OpenSuse、CentOS、Debian 等等。

TC 是 Linux 系统中的一个用户态工具，全名为 Traffic Control (流量控制)。TC 可以用来控制 Netem 模块的工作模式，也就是说如果想使用 Netem 需要至少两个条件，一是内核中的 Netem 模块被启用，另一个是要有对应的用户态工具 TC
**/
Netem 

    

# 二，在linux下设置永久路由的方法：在/etc/rc.local文件中添加以下条目
route add -net 192.168.3.0/24 dev eth0 
route add -net 192.168.2.0/24 gw 192.168.2.254
    
# ip 用法
# 查看 can 设置帮助
ip link set can0 type can help
```

### 03.查看硬件信息

```bash
uname -a # 查看内核/操作系统/CPU信息(含x86_64表示32位机器,i686表示32位机器)
head -n 1 /etc/issue # 查看操作系统版本，是数字1不是字母L
cat /proc/cpuinfo # 查看CPU信息的linux系统信息命令
hostname # 查看计算机名的linux系统信息命令
lspci -tv # 列出所有PCI设备
lsusb -tv # 列出所有USB设备的linux系统信息命令
lsmod # 列出加载的内核模块
env # 查看环境变量资源
free -m # 查看内存使用量和交换区使用量
df -h # 查看各分区使用情况
du -sh # 查看指定目录的大小
du --max-depth=1 -h # 目录层数为1，以K，M，G为单位，查看目录大小
sudo du --max-depth=1 -m /usr/lib | sort -nr # 以MB为单位，从大到小排序
grep MemTotal /proc/meminfo # 查看内存总量
grep MemFree /proc/meminfo # 查看空闲内存量
uptime # 查看系统运行时间、用户数、负载
cat /proc/loadavg # 查看系统负载磁盘和分区
mount | column -t # 查看挂接的分区状态
fdisk -l # 查看所有分区
swapon -s # 查看所有交换分区
hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备)
dmesg | grep IDE # 查看启动时IDE设备检测状况网络
ifconfig # 查看所有网络接口的属性
iptables -L # 查看防火墙设置
route -n # 查看路由表
netstat -lntp # 查看所有监听端口
netstat -antp # 查看所有已经建立的连接
netstat -s # 查看网络统计信息进程
ps -ef # 查看所有进程
top # 实时显示进程状态用户
w # 查看活动用户
id # 查看指定用户信息
last # 查看用户登录日志
cut -d: -f1 /etc/passwd # 查看系统所有用户
cut -d: -f1 /etc/group # 查看系统所有组
crontab -l # 查看当前用户的计划任务服务
chkconfig –list # 列出所有系统服务
chkconfig –list | grep on # 列出所有启动的系统服务程序
rpm -qa # 查看所有安装的软件包
cat /proc/cpuinfo ：查看CPU相关参数的linux系统命令
cat /proc/partitions ：查看linux硬盘和分区信息的系统信息命令
cat /proc/meminfo ：查看linux系统内存信息的linux系统命令
cat /proc/version ：查看版本，类似uname -r
cat /proc/ioports ：查看设备io端口
cat /proc/interrupts ：查看中断
cat /proc/pci ：查看pci设备的信息
cat /proc/swaps ：查看所有swap分区的信息 

# cpu
cat /proc/cpuinfo |grep "model name" && cat /proc/cpuinfo |grep "physical id" 
# 内存
cat /proc/meminfo |grep MemTotal
# 硬盘大小及格分区使用情况 
fdisk -l |grep Disk

# 社区维护版 manual tldr  Too Long Don't Read
tldr tar  # 也可以查看 web网页
```

### 04.嵌入式

#### 4.1交叉编译

```bash
## 打印出当前 ARM 编译器所使用的 include 路径
echo 'main(){}'|/opt/fsl-imx-xwayland/5.4-zeus/sysroots/x86_64-pokysdk-linux/usr/bin/aarch64-poky-linux/aarch64-poky-linux-g++ -E -v -

# 查看 gcc 预处理 C 时的搜索目录
# 若想查看 C++ 或者 clang 可以将 C 修改为 C++ 或者 clang
echo | gcc -x c -v -E -  
#----------------------------------------------------------------------------#
# 交叉编译文件传输采坑记录
#----------------------------------------------------------------------------#
# 使用飞凌交叉编译工具链编译的可执行文件tree，通过不同的传输方式传输至板卡，会出现不同的结果
# 1.<无法执行，提示格式错误>通过 filezilla 直接传输，后 chmod+x 添加可执行权限
file tree：ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, interpreter *empty*, stripped
# 2.<可执行>通过 filezilla 传输 压缩包，后解压执行
file tree：ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=0c126f1b17601d33513b52c02a5e63358d912512, for GNU/Linux 3.14.0, with debug_info, not stripped
# 3.<可执行>通过U盘或者TF卡拷贝
file tree：ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=0c126f1b17601d33513b52c02a5e63358d912512, for GNU/Linux 3.14.0, with debug_info, not stripped
# 使用 ARM 官方下载的 GNU 交叉编译工具链 三种方式均能正确执行(仅限不带链接库)
file a.out：ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, for GNU/Linux 3.7.0, with debug_info, not stripped
# 即使是ARM官方编译器，当用到动态链接库时，直接通过 filezilla 传输，会出现 访问内存超限错误（segmentation fault）
#---------------------------------------------------------------------------------#
```



```bash
# 往SD卡中烧写镜像，命令行操作
# step1： 查找SD卡，从输出可知 系统将 SD卡 分配在 /dev/sdb
fzy@fzy-KPL-W0X:~$ dmesg |tail|awk '$3 == "sd" {print}'
<< 'COMMENT'  # shell 批量注释
[ 7109.144038] sd 2:0:0:0: Attached scsi generic sg1 type 0
[ 7109.391817] sd 2:0:0:0: [sdb] 62333952 512-byte logical blocks: (31.9 GB/29.7 GiB)
[ 7109.391993] sd 2:0:0:0: [sdb] Write Protect is off
[ 7109.391997] sd 2:0:0:0: [sdb] Mode Sense: 03 00 00 00
[ 7109.392142] sd 2:0:0:0: [sdb] No Caching mode page found
[ 7109.392148] sd 2:0:0:0: [sdb] Assuming drive cache: write through
[ 7109.427811] sd 2:0:0:0: [sdb] Attached SCSI removable disk
COMMENT
# step2 写入镜像
/usr/bin/unzip -p ~/Downloads/jetson_nano_devkit_sd_card.zip | sudo /bin/dd of=/dev/sd<x> bs=1M status=progress
# step3 弹出SD卡
sudo eject /dev/sd<x>   # 在本例中为 sudo eject /dev/sdb
```









### 05. Docker

```bash
# 查看当前安装镜像
docker images
# 列出当前容器
docker ps -a
# 删除镜像
docker rmi <XXX>
	
#列出所有的容器 ID
docker ps -aq
#停止所有的容器
docker stop $(docker ps -aq)s
#删除所有的容器
docker rm $(docker ps -aq)
#删除所有的镜像
docker rmi $(docker images -q)
#复制文件
docker cp mycontainer:/opt/file.txt /opt/local/
docker cp /opt/local/file.txt mycontainer:/opt/

# 导出镜像
# https://www.hangge.com/blog/cache/detail_2411.html
docker ps -a
# f299f501774c：容器ID
# hangger_server.tar：保存镜像名
docker export f299f501774c > hangger_server.tar
# 导入镜像
# new_hangger_server： 新导入镜像名
docker import - new_hangger_server < hangger_server.tar
```



```bash
# Docker run
Usage: docker run [OPTIONS] IMAGE [COMMAND] [ARG...]  
 
  -d, --detach=false         指定容器运行于前台还是后台，默认为false   
  -i, --interactive=false   打开STDIN，用于控制台交互  
  -t, --tty=false            分配tty设备，该可以支持终端登录，默认为false  
  -u, --user=""              指定容器的用户  
  -a, --attach=[]            标准输入输出流和错误信息（必须是以非docker run -d启动的容器）
  -w, --workdir=""           指定容器的工作目录 
  -c, --cpu-shares=0        设置容器CPU权重，在CPU共享场景使用  
  -e, --env=[]               指定环境变量，容器中可以使用该环境变量  
  -m, --memory=""            指定容器的内存上限  
  -P, --publish-all=false    指定容器暴露的端口  
  -p, --publish=[]           指定容器暴露的端口 
  -h, --hostname=""          指定容器的主机名  
  -v, --volume=[]            给容器挂载存储卷，挂载到容器的某个目录  
  --volumes-from=[]          给容器挂载其他容器上的卷，挂载到容器的某个目录
  --cap-add=[]               添加权限，权限清单详见：http://linux.die.net/man/7/capabilities  
  --cap-drop=[]              删除权限，权限清单详见：http://linux.die.net/man/7/capabilities  
  --cidfile=""               运行容器后，在指定文件中写入容器PID值，一种典型的监控系统用法  
  --cpuset=""                设置容器可以使用哪些CPU，此参数可以用来容器独占CPU  
  --device=[]                添加主机设备给容器，相当于设备直通  
  --dns=[]                   指定容器的dns服务器  
  --dns-search=[]            指定容器的dns搜索域名，写入到容器的/etc/resolv.conf文件  
  --entrypoint=""            覆盖image的入口点  
  --env-file=[]              指定环境变量文件，文件格式为每行一个环境变量  
  --expose=[]                指定容器暴露的端口，即修改镜像的暴露端口  
  --link=[]                  指定容器间的关联，使用其他容器的IP、env等信息  
  --lxc-conf=[]              指定容器的配置文件，只有在指定--exec-driver=lxc时使用  
  --name=""                  指定容器名字，后续可以通过名字进行容器管理，links特性需要使用名字  
  --net="bridge"             容器网络设置:
                                bridge 使用docker daemon指定的网桥     
                                host     //容器使用主机的网络  
                                container:NAME_or_ID  >//使用其他容器的网路，共享IP和PORT等网络资源  
                                none 容器使用自己的网络（类似--net=bridge），但是不进行配置 
  --privileged=false         指定容器是否为特权容器，特权容器拥有所有的capabilities  
  --restart="no"             指定容器停止后的重启策略:
                                no：容器退出时不重启  
                                on-failure：容器故障退出（返回值非零）时重启 
                                always：容器退出时总是重启  
  --rm=false                 指定容器停止后自动删除容器(不支持以docker run -d启动的容器)  
  --sig-proxy=true           设置由代理接受并处理信号，但是SIGCHLD、SIGSTOP和SIGKILL不能被代理
```

### 06. Git

```bash
# 初始化 git
git init
# 创建、查看、删除分支
git branch

git branch -d <branch> # 删除分支
git push origin --delete remoteBranchName # 删除远程分支
# 切换分支
git checkout
# 查看状态
git status
# 查看 log日志
git log
# 添加文件至暂存区
git add <file>
# 提交至本地仓库
git commit -m "说明"
# 推送至远程仓库，如 github
git push
# 从远程仓库同步回本地
git pull
# 从远程仓库下载资源
git clone
# 本次修改的文件不保存，退回上次提交状态
git restore <file name>
# 删除本次 commit，退回至上次 commit 状态
# git commit后，想撤销 commit
git reset --soft HEAD^
# 若只是想修改 commit 注释
git commit --amend   # 此时会默认进入 vim 编辑器，修改注释后保存即可
# 图形化显示 git下载的程序的版本、分支等各类信息
gitk  

# ------------------------------------------------------------------------------
# pycharm clion等配置 git
# 错误解决GnuTLS recv error (-110): The TLS connection was non-properly terminated
# 参见 https://blog.csdn.net/weixin_43108793/article/details/118306045#commentBox
sudo apt install gnutls-bin
git config --global http.sslVerify false
git config --global http.postBuffer 1048576000

# step1: 登录 GitHub 账号  file/setting/VCS/github
# step2: 配置git ，使用 test自动检测
# step3 菜单栏选择 VCS 为 git
# step4 git/github/create push request,在其中输入你的GitHub仓库链接，后面直接push即可


```

### 07. 驱动开发

```bash
# dtc,编译与反编译 dtb
# 编译 dts 为 dtb
./scripts/dtc/dtc -I dts -O dtb -o tmp.dtb arch/arm/boot/dts/xxx.dts
# 反编译 dtb 为 dts
./scripts/dtc/dtc -I dtb -O dts -o tmp.dts arch/arm/boot/dts/xxx.dtb 
```

### 08. 其他

- /dev/shm（共享内存目录）
  - 这个目录不在硬盘上，而在内存里
  - **默认最大为内存的一半大小**
  - 高速

- 添加库默认搜索路径  /usr/local/lib等
  - 修改 /etc/ld.so.conf，增加 /usr/local/lib
  - ldconfig

