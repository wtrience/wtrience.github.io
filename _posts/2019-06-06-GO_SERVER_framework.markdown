---
layout:     post
title:      "GO Server"
subtitle:   "study note"
date:       2019-09-03 12:00:00
author:     "Tian"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - GO
---


之前c++服务器，是端游转过手游来的，在我不断的优化下，成功从c++98到c++11，以及32位到64位的转变，从端游服务器，转到手游服务器，上线人数单服1W+没太大压力。 go是我业余的时候接触到的，在现在大环境下，高并发和跨平台是现在开发游戏服务器的主流，之前那套c++说实在是够用，只要能达到那个量，单服性能极致，我只服c++。 skynet+lua，c底层，lua逻辑，我也过来过c++底层，lua逻辑，但实际效果是lua并不是我想要去写的，可能我对lua脚本不感兴趣。go做的事情和skynet做的大同小异，都是携程，抢占式g调度模式，go 有个goroutine和channel，skynet lua虚拟机。 当时设计go的时候，由于跟c++大轮子有点出路，c++是循环轮子模式，单逻辑处理能力有目共睹，再设计go的时候我选择了actor模式，每个角色都相互独立，这点携程和信道立功了。全服中，没用到锁的地方不用锁，全服用到锁的地方就一两处，acotr模式立功了，每个角色有各自的消息队列，接受和处理信息。 大体主流网络模式是rpc，不过不是第三方的rpc，而是自己的rpc，不用protbuf那样定义消息体，只需要类型即可，通过refeclt go里面神奇的东西，用好事半功倍，在c++里面只能用宏来实现，或者用template，当然通信方面也支持protobuf客户端与服务器之间的传输。 image.png


go-server
github:https://github.com/bobohume/gonet

gonet 游戏服务器架构，mmo架构，分布式snowflake64为整形uuid,ai行为树，配置data，游戏大部分都在内存运算,分布式缓存redis,增加db模块读取blob数据。

设计之初，建立在actor模式下的；rpc，以及消息驱动，rpc无需注册，支持通用数据(int,[]int,[3]int),map数据,以及struct数据，rpc性能测试如下；sql封装简单的orm(orm支持pb结构体做mysql blob,orm支持结构体做mysql json类型)具体看demo

websocket模式下，要在net，websocket注视掉如下代码,在netgateserver里面注释回//websocket这段

代码除了mysql，protobuf，redis, etcd这几个库以外，其他都是自己写的，方便性能和修改，主动权在自己手里

服务器之间rpc，客户端服务器之间protobuf + rpc，客户端tcp遵从如下消息包头(支持json，考虑到性能，两种传输协议不兼容，请切换json分支)

前四位 protobuf name 的 crc，中间protobuf字节流， 尾部+结束标志💞♡ (结束标志也可以自己定义在base.TCP_END控制)
//另外支持包头大小- 前四位包体大小,再四位protobuf name 的 crc，中间protobuf字节流,代码注视掉,（搜索tcp粘包固定包头）
1.支持go mod, gopath可以不需要设置(使用gomod可以使用goproxy代理(GOPROXY=https://goproxy.i ),不然很坑爹)。（也支持go vendor（删除项目下的go.mod文件），下载这几个基础库，mysql，protobuf，redis，etcd）

// go get github.com/golang/net

// go get github.com/go-sql-driver/mysql

// go get github.com/gomodule/redigo/redis

// go get go.etcd.io/etcd/client

// go get github.com/golang/protobuf

2.下载etcd做服发现（new），（redis做排行榜，全局缓存，可选）

3.bin目录下的sxz_server.cfg配置数据库以及端口

4.数据库在sql文件目录下生产

5.win下执行build.bat,start.bat

6.linux下执行build.sh,start.sh

有问题可以加qq群：950288306

目前游戏库分类：

1.actor核心库，actor模式的雏形。

2.base基础库，分装rpc以及其他基础库。

3.db库，mysql，支持简单orm，没有重度gorm，更加轻便，还在受gorm 0 nil “” 数据库更新就失败的痛苦吗。还在忍受重度gorm带来sql语句都不知道怎么写，没错这个是轻度的。

4.message库，pb用于传输协议。

5.nework库，网络库，tcp，websocket网络管理。rd库，redis库，做一些集群唯一缓存用。

6.client，测试客户端源码，包括go和lua的源码

目前游戏模块：

1.account账号服务，提供注册账号，登录校验，集群服务。

2.natgate网关服务，所有对外都是通过网关转发，集群服务。

3.world世界服务，所有逻辑，集群服务。

4.第三方中间件：etcd分布式服发现，redis分布式缓存。

服务器架构如下： image
