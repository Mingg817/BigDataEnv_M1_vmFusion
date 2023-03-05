链接: https://pan.baidu.com/s/1MdmFBgkTkZmpnQWmnRfcow?pwd=6nge 提取码: 6nge 

# 说明

Hadoop+Hive，启动即用

参考视频 [黑马程序员大数据Hadoop入门视频教程，适合零基础自学的大数据Hadoop教程] (https://www.bilibili.com/video/BV1CU4y1N7Sh)]，由于原视频使用的是x86，修改了的地方还是挺多的

使用VMFusion搭建好的模拟集群，存在三个虚拟机

只使用M系列处理器的Macbook，Macbook Air M1 2020 + VMFusion专业版 13.0.1 测试通过


# 基本配置

需要在macOS系统的hosts中设置

```hosts
192.168.100.101 node1
192.168.100.102 node2
192.168.100.103 node3
```

vmFushion网络中新建自定网络：开启DHCP，配置子网ip为`192.168.100.0`，所有虚拟机都要连接到这个网络配置

所有虚拟机的`root`密码都为`123`，建议使用FinalShell连接，直接导入链接文件

此外虚拟机中存在一个`hadoop`用户，密码同`root`

# 启动Hadpood

开启所有机器

**确保主机和各虚拟机都可联通**

在node1中启动所有集群

```shell
root@node1:~# start-all.sh 
Starting namenodes on [node1]
Starting datanodes
Starting secondary namenodes [node2]
Starting resourcemanager
Starting nodemanagers
```

尝试访问`http://node1:9870/`检查状态

# 启动Hive

node1中检查docker中的mysql是否启动

```shell
docker ps
```

```shell
root@node1:~# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
28947de82de2   mysql     "docker-entrypoint.s…"   31 minutes ago   Up 33 seconds             mysql
```

node1中启动metastore

```shell
screen -dmS metastore /export/server/apache-hive-3.1.2-bin/bin/hive --service metastore --hiveconf hive.root.logger=DEBUG,console 
```

>   可以查看启动状态
>
>   ```shell
>   screen -r metastore
>   ```
>
>   按`control+a+d`返回

>   Tips:稍等一下再进行下面的步骤

node1中启动hiveserver2服务

```shell
screen -dmS hiveserver2 /export/server/apache-hive-3.1.2-bin/bin/hive --service hiveserver2
```

使用DataGrip连接Hive

地址:`192.168.100.101`或者`node`，端口默认，用户名`root`，不填密码

>   底层元数据使用的mysql数据库
>
>   root密码为`hadoop`
