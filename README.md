# Big Data之處理與分析實務班
==========================

## Slides

- https://www.slideshare.net/secret/jAXKRnPCrknokG
- https://www.slideshare.net/secret/zkzZx4ZMLAX4WF
- https://www.slideshare.net/secret/dfD0p9YJG3SfOP

## Centos 6.6 檔案下載

- https://drive.google.com/a/largitdata.com/file/d/0BwcmldsH2om-T3dQS2V3QklmNHM/view?usp=sharing


## 安裝步驟影片
---------------------------------------

### CentOS 安裝
- https://www.youtube.com/watch?v=RkC16DNcYGI&feature=youtu.be

### Ambari Server 安裝步驟
- https://youtu.be/lFBIY_687xY

## 安裝步驟文字

### prepare machine
- su - 
- ssh-keygen
- cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
- ssh localhost

### setup hostname
- ifconfig
- vi /etc/hosts
- hostname master
- hostname -f

### setup ntp
- chkconfig ntpd on
- service ntpd start

### Java Download
- http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u79-oth-JPR
- (選項) https://mirror.its.sfu.ca/mirror/CentOS-Third-Party/NSG/common/x86_64/
- (選項) http://192.168.32.100/jdk-7u79-linux-x64.rpm
- rpm –ivh jdk-7u79-linux-x64.rpm

### 於安裝主機上建立軟連結
- ln -s /usr/java/jdk1.7.0_79 /usr/java/java

### 於安裝主機設定環境變數
- vi /etc/profile 

```
export JAVA_HOME=/usr/java/java
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib/rt.jar
export PATH=$PATH:$JAVA_HOME/bin
```

### 立即更新
- source /etc/profile

### 檢查 Java 版本
- java -version


### 於安裝主機關閉防火牆
- chkconfig iptables off
- service iptables stop

### 於安裝主機設定SELlinux
- vi /etc/selinux/config 將 SELINUX=disabled
- setenforce 0

### HDP建議關閉 Transparent Huge Pages，於安裝主機上執行
- echo never > /sys/kernel/mm/redhat_transparent_hugepage/enabled
- echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag

### 確定登入帳號為 root 在 master 上執行 (需要網路環境好的環境下)
- cd /tmp
- wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.2.0.0/ambari.repo -O /etc/yum.repos.d/ambari.repo 
- yum repolist
- (選項) yum install ambari-server
- (建議選項) wget http://192.168.32.100/ambari-server-2.2.0.0-1310.x86_64.rpm
- (建議選項) yum localinstall ambari-server-2.2.0.0-1310.x86_64.rpm

### 設定 Ambari
- ambari-server setup

### 安裝 Ambari
- ambari-server start

### 連線至Server
- 192.168.81.128:8080

### private key
- cat ~/.ssh/id_rsa


### 設定Repo
- cd /tmp
- wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.2.0.0/ambari.repo -O /etc/yum.repos.d/ambari.repo 
- yum repolist


### 使用local repo
- 將url 更改成192.168.32.106
- https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.1.0/bk_Installing_HDP_AMB/content/_hdp_23_repositories.html


### 修改 replication
- HDFS　-> block replication -> 1

## 建議設定
- wget http://public-repo-1.hortonworks.com/HDP/tools/2.3.0.0/hdp_manual_install_rpm_helper_files-2.3.0.0.2557.tar.gz
- tar zxvf hdp_manual_install_rpm_helper_files-2.3.0.0.2557.tar.gz


## 使用Hive View
- Services > HDFS > Configs.

- Custom core-site -> Click Add Property:
```
hadoop.proxyuser.root.groups=*
hadoop.proxyuser.root.hosts=*
```

## leave safemode
- su - hdfs
- hadoop dfsadmin -safemode leave

##設定權限
- su - hdfs
- hadoop fs -mkdir /user/admin
- hadoop fs -chown admin:hadoop /user/admin


## get wiki count
- su - hdfs
- wget https://dumps.wikimedia.org/other/pagecounts-raw/2007/2007-12/pagecounts-20071209-180000.gz
- gunzip pagecounts-20071209-180000.gz


## setup hue

- hadoop.proxyuser.hue.hosts * 
- hadoop.proxyuser.hue.groups * 
- hadoop.proxyuser.hcat.groups * 
- hadoop.proxyuser.hcat.hosts * 
- hadoop.proxyuser.root.groups * 
- hadoop.proxyuser.root.hosts * 
- hadoop.proxyuser.ambariusr.groups * 
- hadoop.proxyuser.ambariusr.hosts *  
- 確定 hdfs-site.xml 的 webhdfs 是 enabled 
- oozie.service.ProxyUserService.proxyuser.hue.hosts * 
- oozie.service.ProxyUserService.proxyuser.hue.groups *
- webhcat.proxyuser.hue.hosts 是* 
- webhcat.proxyuser.hue.groups 是*


## setup ini 
- vi /etc/hue/conf/hue.ini
- webhdfs_url=http://master:50070/webhdfs/v1/
- /etc/init.d/hue restart

### eclipse (hadoop)
- wget http://eclipse.stu.edu.tw/technology/epp/downloads/release/mars/2/eclipse-java-mars-2-linux-gtk-x86_64.tar.gz
- tar -zxvf eclipse-java-mars-2-linux-gtk-x86_64.tar.gz
- cd eclipse 
- ./eclipse

### eclipse include jar
- a. /usr/hdp/2.3.4.0-3485/hadoop/client/*.jar
- b. /usr/hdp/2.3.4.0-3485/hadoop-mapreduce/*.jar

### Process Data
- head part-r-00000
- cat part-r-00000 | sort -k 2 -nr | head
- cat part-r-00000 | sort -k 2 -nr | awk '{if(length($0)>10) print $0}' | head


### link right java version (root)
- su - 
- rm /usr/bin/java
- ln -s /usr/java/java/bin/java /usr/bin/java
- java -version

### move jar
- su - 
- mv wc* /home/hdfs/
- chown hdfs:hdfs -R /home/hdfs/wc*
- su - hdfs
- hadoop jar wc.jar /tmp/wc.txt /tmp/out
