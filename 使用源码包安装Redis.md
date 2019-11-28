### Redis 服务端的安装

#### 下载和安装

```
wget -c http://download.redis.io/releases/redis-5.0.4.tar.gz
tar zxvf redis-5.0.4.tar.gz
cd redis-5.0.4.tar.gz
make PREFIX=/usr/local install

#复制配置文件到
cp redis.conf /etc/redis_6379.conf
```

#### 修改配置和设置密码

```
# 修改/etc/redis.conf 配置文件
# 使用默认的参数则不需要修改

supervised systemd # 默认为no，改为systemd以守护进程运行 （使用Service启动方式必需修改）
requirepass 123456 # 你的密码
port 6379 # 6379为默认端口号
databases 16 # 默认的数据库是DB 0
```

#### 两种启动方式

##### 1. 命令启动

```
/usr/bin/redis-server /etc/redis_6379.conf
```

##### 2. Service启动（开机自启和后台进程守护）

- 新增文件/usr/lib/systemd/system/redis.service 添加如下代码

```
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=root
Group=root
Type=notify
ExecStart=/usr/local/bin/redis-server /etc/redis_6379.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

- 使service生效和开机自启

```
systemctl daemon-reload
systemctl enable redis.service

# 手动使用命令（redis = redis.service）
systemctl start redis # 开启
systemctl stop redis # 关闭
systemctl restart redis # 重启
```

### Redis 客户端的使用

#### 命令行进入

```
redis-cli
auth {pwd}
或者
redis-cli -a {pwd}
```

> 1.  {}表示变量，例如{pwd}是先前设置的redis的密码
> 2.  进入到redis客户端后不确定是否有密码可使用 " keys * " 验证，有密码并未输入的时候会提示：(error) NOAUTH Authentication required.
> 3.  具体Redis使用命令可参照：http://redisdoc.com/

#### Redis图形化界面

- Redis Desktop Manager
- Redis Studio https://github.com/cinience/RedisStudio/releases

> 此类工具较多，请自行查找搜索，挑选自己适用的管理工具