[![CI](https://github.com/QQxiaoming/virte/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/QQxiaoming/virte/actions/workflows/ci.yml)
[![CodeFactor](https://www.codefactor.io/repository/github/qqxiaoming/virte/badge)](https://www.codefactor.io/repository/github/qqxiaoming/virte)

# virte

## 介绍

这是一个软件模拟的ETH网卡设备驱动，加载驱动后，会产生eth0和eth1两个设备，不同的应用程序分别打开这两个网卡设备可以互相通信(注意要使用linux的网络命名空间隔离两个设备以免进入本地lo网络设备)。

## 安装

```shell
make
make load
```
## 配置

使用网络命名空间分别配置两个网卡，注意替换eth0/eth1为你的网卡名称

```shell
ip netns add eth0spaces
ip link set eth0 netns eth0spaces
ip netns exec eth0spaces ifconfig eth0 192.168.1.100 netmask 255.255.255.0 up

ip netns add eth1spaces
ip link set eth1 netns eth1spaces
ip netns exec eth1spaces ifconfig eth1 192.168.1.101 netmask 255.255.255.0 up
```

## 示例

以下为ping包测试示例

```shell
ip netns exec eth0spaces ifconfig
ip netns exec eth1spaces ifconfig
ip netns exec eth1spaces ping 192.168.1.100
ip netns exec eth0spaces ping 192.168.1.101
```

## 卸载

```shell
make unload
```

删除网络命名空间

```shell
ip netns del eth0spaces
ip netns del eth1spaces
```
