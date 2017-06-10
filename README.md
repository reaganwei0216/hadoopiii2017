# Big Data���B�z�P���R��ȯZ
==========================

## Slides

- https://www.slideshare.net/secret/jAXKRnPCrknokG
- https://www.slideshare.net/secret/zkzZx4ZMLAX4WF
- https://www.slideshare.net/secret/dfD0p9YJG3SfOP

## Centos 6.6 �ɮפU��

- https://drive.google.com/a/largitdata.com/file/d/0BwcmldsH2om-T3dQS2V3QklmNHM/view?usp=sharing


## �w�˨B�J�v��
---------------------------------------

### CentOS �w��
- https://www.youtube.com/watch?v=RkC16DNcYGI&feature=youtu.be

### Ambari Server �w�˨B�J
- https://youtu.be/lFBIY_687xY

## �w�˨B�J��r

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
- (�ﶵ) https://mirror.its.sfu.ca/mirror/CentOS-Third-Party/NSG/common/x86_64/
- (�ﶵ) http://192.168.32.100/jdk-7u79-linux-x64.rpm
- rpm �Vivh jdk-7u79-linux-x64.rpm

### ��w�˥D���W�إ߳n�s��
- ln -s /usr/java/jdk1.7.0_79 /usr/java/java

### ��w�˥D���]�w�����ܼ�
- vi /etc/profile 

```
export JAVA_HOME=/usr/java/java
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib/rt.jar
export PATH=$PATH:$JAVA_HOME/bin
```

### �ߧY��s
- source /etc/profile

### �ˬd Java ����
- java -version


### ��w�˥D������������
- chkconfig iptables off
- service iptables stop

### ��w�˥D���]�wSELlinux
- vi /etc/selinux/config �N SELINUX=disabled
- setenforce 0

### HDP��ĳ���� Transparent Huge Pages�A��w�˥D���W����
- echo never > /sys/kernel/mm/redhat_transparent_hugepage/enabled
- echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag

### �T�w�n�J�b���� root �b master �W���� (�ݭn�������Ҧn�����ҤU)
- cd /tmp
- wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.2.0.0/ambari.repo -O /etc/yum.repos.d/ambari.repo 
- yum repolist
- (�ﶵ) yum install ambari-server
- (��ĳ�ﶵ) wget http://192.168.136.1/ambari-server-2.2.0.0-1310.x86_64.rpm
- (��ĳ�ﶵ) wget http://192.168.31.1/ambari-server-2.2.0.0-1310.x86_64.rpm
- (��ĳ�ﶵ) yum localinstall ambari-server-2.2.0.0-1310.x86_64.rpm

### �]�w Ambari
- ambari-server setup

### �w�� Ambari
- ambari-server start

### �s�u��Server
- 192.168.81.128:8080

### private key
- cat ~/.ssh/id_rsa


### �]�wRepo
- cd /tmp
- wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.2.0.0/ambari.repo -O /etc/yum.repos.d/ambari.repo 
- yum repolist


### �ϥ�local repo
- �Nurl ��令192.168.32.106
- https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.1.0/bk_Installing_HDP_AMB/content/_hdp_23_repositories.html


### �ק� replication
- HDFS�@-> block replication -> 1

## ��ĳ�]�w
- wget http://public-repo-1.hortonworks.com/HDP/tools/2.3.0.0/hdp_manual_install_rpm_helper_files-2.3.0.0.2557.tar.gz
- tar zxvf hdp_manual_install_rpm_helper_files-2.3.0.0.2557.tar.gz


## �ϥ�Hive View
- Services > HDFS > Configs.

- Custom core-site -> Click Add Property:
```
hadoop.proxyuser.root.groups=*
hadoop.proxyuser.root.hosts=*
```

## leave safemode
- su - hdfs
- hadoop dfsadmin -safemode leave

##�]�w�v��
- su - hdfs
- hadoop fs -mkdir /user/admin
- hadoop fs -chown admin:hadoop /user/admin


## get wiki count
- su - hdfs
- wget https://dumps.wikimedia.org/other/pagecounts-raw/2007/2007-12/pagecounts-20071209-180000.gz
- gunzip pagecounts-20071209-180000.gz


## setup hue

- hadoop.proxyuser.hue.hosts * p
- hadoop.proxyuser.hue.groups * p
- hadoop.proxyuser.hcat.groups * p
- hadoop.proxyuser.hcat.hosts * p
- hadoop.proxyuser.root.groups * p
- hadoop.proxyuser.root.hosts * p
- hadoop.proxyuser.ambariusr.groups * p
- hadoop.proxyuser.ambariusr.hosts * n 
- �T�w hdfs-site.xml �� webhdfs �O enabled 
- oozie.service.ProxyUserService.proxyuser.hue.hosts * p
- oozie.service.ProxyUserService.proxyuser.hue.groups *
- webhcat.proxyuser.hue.hosts �O* p
- webhcat.proxyuser.hue.groups �O*


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
