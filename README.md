# put-linux-autoupdate-onhold
关闭ubuntu,Debian,CentOS系统的自动更新  
   
前段时间有些云主机玩家的小硬盘鸡文件空间爆了,但是他们刚开始装小鸡的时候没有这个问题的,其中原因主要是linux系统的自动更新功能自动定期下载更新包,日积月累的就慢慢把硬盘文件空间挤爆了,下面就把关闭各种linux系统自动更新的方法分享给各位拥有小硬盘鸡的玩家   
  
执行下面的步骤前建议先执行update及upgrade命令,具体每个系统如何打这些命令请自行找度娘去插    
   
------------ubuntu 18/20 关闭更新------------    
    
转用root用户  
  
$su  
#cd /  
  
编辑以下两个文件,将里面所有的参数都改为0   
#nano /etc/apt/apt.conf.d/10periodic   
   
#nano /etc/apt/apt.conf.d/20auto-upgrades   
   
查看packagekit服务文件,如果有的话就执行停止服务命令   
#cat /lib/systemd/system/packagekit.service   
   
停止packagekit服务调用apt进行更新   
#systemctl disable packagekit.service   
   
关闭内核更新   
   
#apt-mark hold linux-image-generic linux-headers-generic
  
重启机器  
#reboot   
   
------------Debian 10 关闭更新------------   
   
转用root用户  
  
$su  
#cd /  
  
检查系统是否已安装unattended-upgrades包（没有安装则无信息返回）,如果有则跳过安装包的步骤   
   
#dpkg -l | grep unattended-upgrades   
   
安装unattended-upgrades包   
   
#apt -y update && apt-get install unattended-upgrades   
   
启动服务配置向导用以生成配置文件  
   
#dpkg-reconfigure --priority=low unattended-upgrades   
选yes生成20auto-upgrades配置文件   
    
编辑20auto-upgrades文件,将里面所有的参数都改为0   
   
#nano /etc/apt/apt.conf.d/20auto-upgrades   
   
保存后退出   
   
再次启动服务配置向导  
   
#dpkg-reconfigure --priority=low unattended-upgrades   
选no设定不自动更新  
  
重启机器  
  
#reboot  
  
------------CentOS 7关闭更新------------   

转用root用户  
  
$su  
#cd /  
  
首先安装yum-cron包  
  
#yum install -y yum-cron  
  
修改yum-cron.conf文件  
  
#nano /etc/yum/yum-cron.conf  
  
把update_messages = yes的值改为no  
  
把download_updates = yes的值改为no     
  
保存并退出  
  
设置开机启动yum-cron服务  
#systemctl enable yum-cron  
  
重启yum-cron服务  
#systemctl restart yum-cron  
  
  
------------CentOS 8关闭更新------------   
   
转用root用户  
  
$su  
#cd / 
  
安装dnf-automatic包  
#dnf install dnf-automatic -y  
  
修改automatic.conf文件  
#nano /etc/dnf/automatic.conf  
  
把download_updates = yes的值改为no  
  
保存并退出  
  
立即启动服务  
#systemctl enable --now dnf-automatic.timer  
  





