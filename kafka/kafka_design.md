[TOC]

监控、权限

数据流转、配置



## kafka使用
### Zookeeper安装

* 下载zookeeper
* 解压： tar xvf zookeeper-3.4.10.tar.gz
* 修改配置
  配置文件/zookeeper/zookeeper-3.4.10/conf/zoo.cfg
    + 路径配置
    > dataDir=/usr/local/zookeeper/data

    - dataDir为存储快照文件的目录，默认情况下，事务日志也会存储在该目录上。由于事务日志的写性能直接影响 ZooKeeper 性能，因此 建议同时配置参数dataLogDir
  > dataLogDir=/opt/data/zookeeper/logs

* 集群配置
  首先在 3 台机器的/etc/hosts 文件中加入 3 台机器 的 IP 与机器域名映射， 域名自定义， 这里分别命名为 server-I、 server-2、 server-3, 3 台机器 IP与机器域名映射关 系如下:
    
    >10.211.55.4 server-1    
    >10.211.55.5 server-2    
    >10.211.55.6 server-3    

    进入其中一台机器的Zookeeper安装路径conf，添加
    >server.1=server-1:2888:3888    
    >server.2=server-2:2888:3888    
    >server.3=server-3:2888:3888

    端口号2888表示该服务器与集群中leader交换信息的端口，默认为2888， 3888表示选举时服务器相互通信的端口。

    接着在${dataDir}路径下创建一个myid文件，myid存放的值就是服务器的编号，即对应上面的1、2、3。ZooKeeper在启动时会读取 myid文件 中的值与 zoo.cfg文件中的配置信息进行比较， 以确定是哪台服务器。

    将配置好的zoo.cfg拷贝到其他两台机器，并分别创建对应的myid。
    
    为了操作方便，我们可以将Zookeeper相关环境变量添加到/etc/profile文件中，如：
    
    >export ZOOKEEPER_HOME=/usr/local/zookeeper/zookeeper
    PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin:$ZOOKEEPER_HOME/bin

* 验证 

    启动zookepper:  
    zkServer.sh start   
    输出下面类似结果表示安装成功
    ```
    [root@centos7g zookeeper]# zkServer.sh start   
    ZooKeeper JMX enabled by default    
    Using config: /usr/local/zookeeper/zookeeper/bin/../conf/zoo.cfg    
    Starting zookeeper ... STARTED  
    ```
    查看zookepper状态:  
    zkServer.sh status   
    输出下面类似结果表示安装成功
    ```
    [root@centos7g zookeeper]# zkServer.sh status  
    ZooKeeper JMX enabled by default    
    Using config: /usr/local/zookeeper/zookeeper/bin/../conf/zoo.cfg    
    Mode: standalone    
    
     ```
    
    注意： centos7默认使用firewall作为防火墙，并默认开启， 在启动zk时需要关闭防护墙，不然无法通信。    
    查看防火墙状态： firewall-cmd --state   
    停止防火墙： systemctl stop firewalld.service   
    禁止防火墙开机启动：systemctl disable firewalld.service 

### kafka安装
* 安装包    

    下载 kafka：http://kafka.apache.org/downloads
    目前我们使用 Kafka 版本为 kafka_2.11-1.0.0.tgz，其中 2.11 代表 Scala 版本， 1.0.0 表示 Kafka 的版本     
    解压： tar xvf kafka_2.11-1.0.0.tgz

    这里我们对kafka的环境变量进行设置， 在/etc/profile 文件中加入kafka的安装路径，

    >export KAFKA_HOME=/home/xxx/software/kafka_2.12-2.1.0
    PATH=\$PATH:\$JAVA_HOME/bin:\$JRE_HOME/bin:\$ZOOKEEPER_HOME/bin:\$KAFKA_HOME/bin
    
* 修改配置
    修改$KAFKA_HOME/config 目录下的server.properties文件，为了便于后续集群环境搭建的配置， 需要保证同一个集群下 broker.id要唯一，因此这里手动配置 broker.id, 直接保持与ZooKeeper的myid值一致， 同时配置日志存储路径。server.properties修改的配置 如下 :
    >broker.id=l #指定代理的 id     
    >log.dirs=/usr/local/kafka/log  #指定 Log 存储路径  
    >zookeeper.connect=10.200.195.75:2181,server-2:2181,server-3:2181   

    在三台机器上(或者同一台机器三个路径，以端口区分)分别修改配置文件server.properties, 并修改对应的broker.id.

* 启动  
    >kafka-server-start.sh -daemon ../config/server.properties   

    执行 jps命令查看 Java进程，此时进程信息至少包括以下几项:
    
    ```
    15976 Jps
    14999 QuorumPeerMain
    15906 Kafka
    ```

    通过 ZooKeeper 客户端登录 ZooKeeper 查看目录结构，执行以下命令:
    >zkCli.sh -server server 1:2181 #登录 ZooKeeper     
    ls / #查看 ZooKeeper 目录结构   
    ls /brokers/ ids 输出 [1, 2, 3]

    由/brokers/ids 节点存储的元数据可知， 3台机器的 Kafka 均已正常己启动。
    
```
### kafka Demo

创建拥有副本的topic

正常的生产、消费消息

kill leader节点

正常消费消息
```




## Design

http://kafka.apache.org/documentation/#design

更系统化，以入门为基础


模块、组件、集群的通讯。
稳定性相关的。

数据存取的细节，请求的处理，存储的数据结构等
保证每个环节的、可靠性，落到具体的组件，存储格式等。

数据流转图，（汇总，目前图比较粗。细化到partition，以及zookeeper）

>## 配置

>http://kafka.apachecn.org/documentation.html#configuration
>给出建议配置
>常用参数，常用场景
>【扩展：demo】



```

### 配额
（消费权重）

## operation
多数据中心

### 管理
节点管理，起停
topic的管理
多集群的管理

### 监控
http://kafka.apache.org/documentation/#monitoring

### 流式计算
大数据领域
其中一个概念：并行计算


【扩展】

kafka-》es
kafka stream

* 持续输入
* 持续输出

mysql-> kafaka ->es
```




## Reference
[1]. http://kafka.apachecn.org/documentation.html   
[2]. kafka安装，https://www.jianshu.com/p/c74e0ec577b0
