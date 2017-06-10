## �]�w�v��
- ������ root �ϥΪ�

$ su �V

- �ϥ�visudo �s���v��

$ visudo 

- ����sudo �v�� (��99��B)

hadoop	ALL=(ALL)	ALL

## Java �w��

### �����ª�Java
- yum -y remove java-*

### �U���Φw��Java

- http://www.oracle.com/technetwork/java/javase/downloads/index.html

- sudo rpm -ivh jdk-8u45-linux-x64.rpm

### �]�wHadoop

- wget http://ftp.twaren.net/Unix/Web/apache/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz

- tar -zxvf hadoop-2.6.0.tar.gz


### �NHadoop 2.6.0 �h���� /usr/local
- sudo mv hadoop-2.6.0 /usr/local/hadoop

### �s��.bashrc
- $ vim ~/.bashrc
- export JAVA_HOME=/usr/java/jdk1.8.0_45/
- export PATH=$PATH:$JAVA_HOME
- export HADOOP_PREFIX=/usr/local/hadoop 
- export HADOOP_COMMON_HOME=$HADOOP_PREFIX 
- export HADOOP_HDFS_HOME=$HADOOP_PREFIX 
- export HADOOP_MAPRED_HOME=$HADOOP_PREFIX 
- export HADOOP_YARN_HOME=$HADOOP_PREFIX 
- export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_PREFIX/lib/native
- export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib"
- export HADOOP_CONF_DIR=$HADOOP_PREFIX/etc/hadoop 
- export PATH=$PATH:$HADOOP_PREFIX/bin:$HADOOP_PREFIX/sbin

### ��s�ܼ�
$ source ~/.bashrc

### ���լO�_�i�H�L�K�X�n�J
- ssh localhost

### �ק�/etc/ssh/sshd_config
- sudo vi /etc/ssh/sshd_config
- �NPasswordAuthentication?�ܧ�no
- sudo service sshd restart

### �]�m�L�K�X�n�J
- ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
- cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
- chmod 700 ~/.ssh
- chmod 600  ~/.ssh/authorized_keys

### �ק�]�w��
- pseudo-distribution.xml

### �榡��HDFS
- hdfs namenode -format

### �ҥ�HDFS��YARN
- start-dfs.sh
- start-yarn.sh
