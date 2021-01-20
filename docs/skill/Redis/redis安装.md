# [mac系统安装redis](https://www.cnblogs.com/feijl/p/6879929.html)

1.下载

打开官网：https://redis.io/

Download---Stable---Download3.2.8，下载最新稳定版，这里是3.2.8

![img](redis%E5%AE%89%E8%A3%85.assets/1167593-20170519183731135-1346022971.png)

2.安装

下载完成后，打开命令行工具，执行解压命令

```
tar zxvf redis-3.2.8.tar.gz
```

![img](https://images2015.cnblogs.com/blog/1167593/201705/1167593-20170519183634697-986704125.png)

 

将解压后文件夹放到/usr/local

```
mv redis-3.2.8 /usr/local/
```

切换到相应目录

```
cd /usr/local/redis-3.2.8/
```

编译测试

```
sudo make test
```

![img](redis%E5%AE%89%E8%A3%85.assets/1167593-20170519183542619-547056138.png)

 

编译安装

```
sudo make install
```

![img](redis%E5%AE%89%E8%A3%85.assets/1167593-20170519183344838-1229936022.png)

启动Redis

```
redis-server
```

![img](redis%E5%AE%89%E8%A3%85.assets/1167593-20170519184035963-195318073.png)

3.配置

在redis目录下建立bin，etc，db三个目录

```
sudo mkdir  /usr/local/redis-3.2.8/bin
sudo mkdir  /usr/local/redis-3.2.8/etc
sudo mkdir  /usr/local/redis-3.2.8/db
```

把/usr/local/redis/src目录下的mkreleasehdr.sh，redis-benchmark， redis-check-rdb， redis-cli， redis-server拷贝到bin目录

```
cp /usr/local/redis-3.2.8/src/mkreleasehdr.sh .
cp /usr/local/redis-3.2.8/src/redis-benchmark .
cp /usr/local/redis-3.2.8/src/redis-check-rdb .
cp /usr/local/redis-3.2.8/src/redis-cli .
cp /usr/local/redis-3.2.8/src/redis-server .
```

拷贝 redis.conf 到 /usr/local/redis/etc下

```
cp /usr/local/redis-3.2.8/redis.conf /usr/local/redis-3.2.8/etc
```

修改redis.conf

[![复制代码](redis%E5%AE%89%E8%A3%85.assets/copycode.gif)](javascript:void(0);)

```
#修改为守护模式
daemonize yes
#设置进程锁文件
pidfile /usr/local/redis-3.2.8/redis.pid
#端口
port 6379
#客户端超时时间
timeout 300
#日志级别
loglevel debug
#日志文件位置
logfile /usr/local/redis-3.2.8/log-redis.log
#设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
databases 16
##指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
#save <seconds> <changes>
#Redis默认配置文件中提供了三个条件：
save 900 1
save 300 10
save 60 10000
#指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，
#可以关闭该#选项，但会导致数据库文件变的巨大
rdbcompression yes
#指定本地数据库文件名
dbfilename dump.rdb
#指定本地数据库路径
dir /usr/local/redis-3.2.8/db/
#指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能
#会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有
#的数据会在一段时间内只存在于内存中
appendonly no
#指定更新日志条件，共有3个可选值：
#no：表示等操作系统进行数据缓存同步到磁盘（快）
#always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
#everysec：表示每秒同步一次（折衷，默认值）
appendfsync everysec
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

启动服务

```
./bin/redis-server etc/redis.conf
```

查看日志

```
tail -f log-redis.log
```

![img](redis%E5%AE%89%E8%A3%85.assets/1167593-20170519185545557-1241804032.png)

打开redis客户端

```
./bin/redis-cli
```

执行redis命令

![img](redis%E5%AE%89%E8%A3%85.assets/1167593-20170519185743385-1023279574.png)

尽情操作吧。

