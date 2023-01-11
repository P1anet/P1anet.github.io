---
layout: post
title: "Bitfusion测试环境搭建笔记"
subtitle: "Bitfusion Memo"
date: 2023-01-11
author: "Kenshin"
header-img: "img/post-bg/post-bg-default.png"
tags: 
---

![bitfusion集群](/img/in-post/cs_learning/2023-01-11-bitfusion-cluster.png)

采用 C-S 架构，最小 vSphere Bitfusion 集群配置为一个客户端、一个服务器和一个 vCenter Server。

在使用 VMware vSphere Bitfusion 之前，必须部署 vSphere Bitfusion 服务器，并在客户端计算机上安装和激活 vSphere Bitfusion 软件。

bitfusion使用介入技术执行共享：截获寻址本地加速器（PCIe主机总线上）的API调用，然后通过网络发送调用及相关数据

## 组件：
- vSphere Bitfusion 服务器
  - vSphere Bitfusion 服务器在具有本地安装 GPU 的 ESXi 主机上作为 VMware 设备运行，即具有预打包软件和服务的预配置虚拟机 (VM)。服务器需要访问本地 GPU，通常通过 VMware vSphere ® DirectPath I/O™ 访问。
- vSphere Bitfusion 客户端
  - vSphere Bitfusion 客户端在运行 AI 和 ML 应用程序的虚拟机上运行。
- vSphere Bitfusion 集群
  - vSphere Bitfusion 集群是 vCenter Server 实例中所有 vSphere Bitfusion 服务器和客户端的集合。
- vSphere Bitfusion 插件
  - vSphere Bitfusion 服务器将向 VMware vCenter Server 注册 vSphere Bitfusion 插件。该插件可监控和管理 vSphere Bitfusion 客户端与服务器。
- vSphere Bitfusion Linux 用户组
  - 在 vSphere Bitfusion 客户端安装过程中，客户端会创建一个 vSphere Bitfusion Linux 用户组 bitfusion。只有该组的成员才能使用 vSphere Bitfusion。某些配置文件设置有适当的权限，组成员将继承相应的限制，以高效使用 vSphere Bitfusion。
- vSphere Client
  - 通过 vSphere Client，可以使用 Web 浏览器连接到 vCenter Server 实例，以便管理 vSphere 基础架构。您可以通过 vSphere Client 访问 vSphere Bitfusion 插件。
- 命令行界面 (CLI)
  - 您可以使用命令行界面 (CLI) 命令管理 vSphere Bitfusion 服务器和客户端。
- vCenter Server
  - vCenter Server 是服务器管理软件，提供了一个集中式平台来控制您的 vSphere 环境。


## 安装流：

安装服务器→安装客户端→激活客户端（vCenter Server实例，裸机计算机，k8s集群）→测试部署

- 集群要求：一个 vSphere Bitfusion 集群必须包含一个、三个或更多 vSphere Bitfusion 服务器。如果一个集群包含两个服务器，则其中一个服务器无法运行时，可能会丢失多数仲裁。如果集群仅包含两个具有 GPU 的服务器，则可以添加第三个不具有 GPU 的 vSphere Bitfusion 服务器，用作见证服务器。
- 系统要求：https://docs.vmware.com/cn/VMware-vSphere-Bitfusion/4.0/Install-Guide/GUID-A379AA65-8FC9-4878-AEC7-287B6BD67279.html
  - vSphere Bitfusion 需要一个 ESXi 主机，以用于安装 vSphere Bitfusion 服务器。
  - vSphere Bitfusion 需要在虚拟机、裸机计算机或容器上安装 vSphere Bitfusion 客户端。

- 官网下载VMware workstation 16 pro, 安装
  - 以下是VMware Workstation Pro 16激活码
    > ZF3R0-FHED2-M80TY-8QYGC-NPKYF
    > 
    > YF390-0HF8P-M81RQ-2DXQE-M2UT6
    >
    > ZF71R-DMX85-08DQY-8YMNC-PPHV8

- 下载esxi7.0镜像(https://blog.csdn.net/China_yuqin_work/article/details/121285962)
  - 6.7(https://blog.csdn.net/China_yuqin_work/article/details/121282915)
  - 未测试https://new.llycloud.com/2019/12/18/VMware-vSphere-7-vCenter-7-ESXi-7-Free-Download-With-Test-Key/
- 下载bitfusion ova文件(https://customerconnect.vmware.com/downloads/details?downloadGroup=BITFUSION-451&productId=1221&download=true&fileId=82754a742f2878a6b87f6ee187257439&uuId=743b6e5d-88c6-491a-90f9-5cc0f9c94076)
- 证书指纹：8eeefe73bff6d00a77ff38523005184b6983653c，添加到OVF模板的部署属性中
- ...

## 在ubuntu上安装vSphere Bitfusion Client

- 下载 https://packages.vmware.com/bitfusion/ubuntu/
  - 如wget https://packages.vmware.com/bitfusion/ubuntu/18.04/bitfusion-client-ubuntu1804_4.5.0-4_amd64.deb
- vsphere client：https://customerconnect.vmware.com/cn/downloads/details?downloadGroup=NSXV_6413_EDGE&productId=974
  - 管理功能上大于esxi自带管理系统


## 在esxi主机上配置显卡驱动：

- 下载驱动：https://ui.licensing.nvidia.com/software（好像进不去
  - 查看显卡和服务器兼容：https://www.nvidia.com/zh-cn/data-center/resources/vgpu-certified-servers/
- esxi开启ssh（或在vcenter上开）
- 使用xftp或winscp将驱动解压后上传至tmp目录下
- lspci | grep NVIDIA
- esxcli software vib install -v tmp/****.vib #安装驱动注意使用绝对路径
- 重启机器后nvidia-smi
- 关闭ECC：nvidia-smi -e 0
- 配置-图形-编辑，修改显卡为直接共享，重启xorg服务
- 编辑虚拟机为共享PCI设备，nvidia grid vgpu

> 配置windows server2016 + esxi环境，安装vcsa，参考B站教程