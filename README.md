[![CI](https://github.com/QQxiaoming/virte/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/QQxiaoming/virte/actions/workflows/ci.yml)
[![CodeFactor](https://www.codefactor.io/repository/github/qqxiaoming/virte/badge)](https://www.codefactor.io/repository/github/qqxiaoming/virte)
[![License](https://img.shields.io/github/license/qqxiaoming/virte.svg?colorB=f48041&style=flat-square)](https://github.com/QQxiaoming/virte)

# virte

English | [简体中文](./README_zh_CN.md)

## Introduction

This is a software-simulated ETH network card device driver. After loading the driver, two devices, veth0 and veth1, will be generated. Different applications can open these two network card devices to communicate with each other (note that the network namespace of linux should be used to isolate the two devices) device so as not to enter the local loop network device).

## Install

```shell
make
make load
```
## Config

Use the network namespace to configure two NICs separately, pay attention to replace veth0/veth1 with your NIC name

```shell
ip netns add veth0spaces
ip link set veth0 netns veth0spaces
ip netns exec veth0spaces ifconfig veth0 192.168.1.100 netmask 255.255.255.0 up

ip netns add veth1spaces
ip link set veth1 netns veth1spaces
ip netns exec veth1spaces ifconfig veth1 192.168.1.101 netmask 255.255.255.0 up
```

## Example

The following is an example of a ping packet test:

```shell
ip netns exec veth0spaces ifconfig
ip netns exec veth1spaces ifconfig
ip netns exec veth1spaces ping 192.168.1.100
ip netns exec veth0spaces ping 192.168.1.101
```

## Uninstall

```shell
make unload
```

delete network namespace

```shell
ip netns del veth0spaces
ip netns del veth1spaces
```
