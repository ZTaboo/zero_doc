## 动态发现

### 公共发现服务

```bash
curl https://discovery.etcd.io/new?size=3
```

- 配置文件:

```yaml
name: master
heartbeat-interval: 2000
election-timeout: 10000
api:
  api-version: 3
# 动态发现不需要此项
#initial-cluster: "master=http://192.168.1.115:2380,node1=http://192.168.1.132:2381"
discovery: <https://discovery.etcd.io/01a4371c04851af66305daac16cf2a6f>
# 动态发现服务此配置必须是new或者不写
initial-cluster-state: new
initial-cluster-token: zero-etcd
initial-advertise-peer-urls: <http://192.168.1.115:2380>
listen-peer-urls: <http://192.168.1.115:2380>
listen-client-urls: <http://192.168.1.115:8080>
```

## 添加集群角色坑

### 报错:request cluster ID mismatch

解决办法：删除了etcd集群所有节点中的--data_dir配置的数据存储目录下所有数据如：/datasoft/soft/etcd_v3.3.12/default.etcd/ 分析： 因为集群搭建过程，单独启动过单一etcd,做为测试验证，集群内第一次启动其他etcd服务时候，是通过发现服务引导的，所以需要删除旧的成员信息

## 角色控制系统使用

### 特殊用户和角色

一个特殊用户: root

一个特殊角色: root

### 1.root用户

> root用户拥有etcd的所有权限,且必须在激活身份认证之前就创建好. root用户的设计主要是出于管理的目的: 管理角色和普通用户. root用户必须具有root角色, 并且可以在etcd中进行任何修改.


### 2.root角色

```bash
root角色除了可以赋予root用户外, 也可以赋予任何其他用户. 具有root角色的用户具有全局读写访问权限, 并且还具有更新集群的权限. 另外, root角色授予常规集群维护的特权, 包括修改及群成员的资格, 对存储进行碎片整理以及拍摄快照.

激活身份认证

#1. 添加root角色
etcdctl role add root

#2. 添加root用户
etcdctl user add root
(设置root密码)

#3. 给root用户授予root角色
etcdctl user grant-role root root

#4.激活auth
etcdctl auth enable
```

### 用户管理

```bash
etcdctl 的 user 子命令处理与用户账户有关的所有事情.
```

#### 用户列表

```bash
etcdctl user list
```

#### 创建用户

```bash
[root@etcd1-101~]# etcdctl user add 用户名
Password of 用户名:
Type password of 用户名 again for confirmation:
User root created

创建用户时需要提供一个密码，如果使用 --interactive=false选项，支持从标准输入提供，也可以使用 --new-user-password 选项提供
```

#### -new-user-password

```bash
etcdctl --user root:123 user add Alayman --new-user-password 123 # --interactive=false etcdctl --interactive=false --user root:123 user passwd xinwei < /etc/etcd/passwd/xinwei
```

#### 添加和删除用户角色

```bash
为用户添加角色
etcdctl user grant-role 用户名 角色名

为用户删除角色
etcdctl user revoke-role 用户名 角色名
```

### 角色管理

#### 列出角色

```bash
etcdctl role list
```

#### 创建新角色

```bash
etcdctl role add 角色名

角色没有密码，它只是定义了一组新的访问权限。
角色可以授予到一个单一的key上，或则一个范围的keys。
范围可以指定为[开始key，结束key) 区间，其中开始key应该按字母顺序小于词结束key。
```

#### 角色的访问权限可以被赋予read（读）,write（写）,readwrite（读和写）权限

```bash
**# 给 /foo 读权限
etcdctl role grant-permission 角色名 read /foo
```

#### 给以/foo/开头的所有key赋予读权限. The prefix is equal to the range [/foo/, /foo0)

```bash
etcdctl role grant-permission 角色名 --prefix=true read /foo/ # 只给 /foo/bar 赋予写权限
etcdctl role grant-permission 角色名 write /foo/bar
```

#### 给[key1, key5] 范围内的所有key赋予所有权限

```bash
etcdctl role grant-permission 角色名 readwrite key1 key5
```

#### 赋予以/pub/可有的key所有权限

```
etcdctl role grant-permission 角色名 --prefix=true readwrite /pub/**
```

#### 删除一个角色的权限

```
etcdctl role revoke-permission 角色名 /foo/bar
```

#### 删除角色

```bash
etcdctl role delete 角色名
```

#### 添加普通用户权限的步骤

```
1. 添加角色role
2. 给角色授权 role grant-permissoion
3. 添加用户 user
4. 给用户授予角色权限 user grant-role

**#1. 添加角色 role

etcdctl --user root role add xinweiblog

#或者 etcdctl --user root --password (root密码) role add xinweiblog

#或者 etcdctl --user root:(root密码) role add xinweiblog #2. 给角色授权 role grant-permission etcdctl --user root role grant-permission xinweiblog --prefix=true readwrite/xinweiblog

#3. 添加用户 user

etcdctl --user root user add xinwei

(设置密码)

#4. 给用户授予角色权限 user grant-role
etcdctl --user root user grant-role xinweiblog
etcdctl --user root user grant-role xinwei xinweiblog**
```

#### 列出已存在的用户

```
etcdctl --user root:(root密码)  user list
xinwei
root

etcdctl --user root:(root密码) role get xinweiblog

Role xinweiblog

KV Read:

  [/xinweiblog, /xinweibloh) (prefix /xinweiblog)

KV Write:

  [/xinweiblog, /xinweibloh) (prefix /xinweiblog)
```

#### 7.etcdctl --help 中相关命令

```bash
role add        添加角色
role delete        删除角色
role get        获取一个角色的详细信息
role grant-permission    给角色赋予一个key的管理权限
role list        列出所有的角色
role revoke-permission    收回角色对一个key的管理权限
user add        添加用户
user delete        删除用户
user get        获取一个用户的详细信息
user grant-role        给用户赋予一个角色
user list        列出所有的用户
user passwd 修改用户密码
user revoke-role    删除用户的角色
```
