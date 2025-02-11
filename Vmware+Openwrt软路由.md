根本需要：统一代理出口（socks）

目标：虚拟机搭软路由，应对大多数路由组网场景，如路由器-软路由、虚拟机-软路由、本机-软路由

硬件：

- 笔记本
- 家用路由器
- 网线

软件：

- Vmware 
- Openwrt 镜像包 
  - https://downloads.openwrt.org/releases/18.06.2/targets/x86/64/openwrt-18.06.2-x86-64-combined-ext4.img.gz 
  - 官网： https://openwrt.org/
- StarWind V2V Converter 
  - https://www.starwindsoftware.com/starwind-v2v-converter#download
- redsocks
  - openwrt插件

主要参考：

- Openwrt安装配置
  - https://zhuanlan.zhihu.com/p/662650136
- Redsocks+iptables配置
  - [https://anuoua.github.io/2020/05/28/redsocks%E6%90%AD%E9%85%8Diptables%E5%AE%9E%E7%8E%B0%E7%9C%9F%E5%85%A8%E5%B1%80%E4%BB%A3%E7%90%86/](https://anuoua.github.io/2020/05/28/redsocks搭配iptables实现真全局代理/)
  - http://skyy.life/archives/9/

要点：

1、虚拟网卡添加顺序与实体设备类似，第二个为wan口，其他为lan口。核心就是wan口桥接物理机网卡（WIFI），根据需要将其他物理机网卡或虚拟机网卡接lan口。

2、物理机网卡接lan口时需关闭IPv4设置，避免以外接设备为路由

<img src="imgs\1739269350021.jpg" alt="1739269350021" style="zoom: 33%;" />

3、物理机走软路由时，也需关闭wan口桥接网卡的IPv4

4、iptables配置参考：

```
iptables -t nat -N REDSOCKS

iptables -t nat -I REDSOCKS -s xx.xx.xx.xx

iptables -t nat -A REDSOCKS -p tcp -j REDIRECT --to-ports 12345

iptables -t nat -I PREROUTING -p tcp -j REDSOCKS

iptables -t nat -A REDSOCKS -d 127.0.0.1 -j RETURN

iptables -t nat -A REDSOCKS -d 127.0.0.0/8 -j RETURN

iptables -t nat -A REDSOCKS -d 192.168.0.0/16 -j RETURN
```

 

另外，openwrt 22.03后防火墙弃用iptables，使用nftables。要方便直接上手，可参考文档使用老版openwrt。

待做：

整理udp转发方式