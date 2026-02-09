<h1 style="text-align: center;">OpenClash & Clash Verge Configuration</h1>

自用clash配置，适用于 openwrt([immortalwrt](https://github.com/immortalwrt/immortalwrt)) 上的 [OpenClash](https://github.com/vernesong/OpenClash) 和 终端 上的 [clash-verge-rev](https://github.com/clash-verge-rev/clash-verge-rev)

实现在订阅中分流，前者可以使用Geo数据库，而后者只能在订阅中拉远程的list，所以有所不同

<br></br>

## 1.OpenClash

### 1.1 概述与准备

[OpenClash配置示例及订阅转换模板，无需套娃，无泄漏](https://github.com/Aethersailor/Custom_OpenClash_Rules)

先参照上述仓库wiki进行配置

注意先后顺序，推荐先设置Geo相关的数据，趁你的网络还能访问GitHub，否则若按照wiki中设置，重启后可能无法成功，因为没先下载Geo相关的数据。

先更新clash客户端和内核到最新发行版

1. GEO 数据库订阅，全点一遍检查并更新，都会重启一次clash
1. 覆写-meta-启用 GeoIP Dat 版数据库，勾选后应用，会重启clash下载这个dat数据

完成这两个步骤再按wiki从头设置。

<br></br>

以上设置实现了dns无套娃，采用的是运营商dns服务器

接下来是实现数据流量分流，不同应用流量走不同的线路

有两种方案：

1. 远程服务器+远程ini模板文件实现订阅转换。参考本章开头链接。
2. 使用配置文件覆写，远程拉取yaml文件覆写。参考[OpenClash_Overwrite](https://github.com/Giveupmoon/OpenClash_Overwrite)，先复制conf文件到覆写，其中包含了远程拉取yaml文件内容。

<br></br>

### 1.2 订阅转换

编辑订阅

在线订阅转换

服务器api.asailor.org

模板拉到最下方选自定义

```
https://testingcf.jsdelivr.net/gh/Charles94jp/Clash@master/open_clash.ini
```

启用：UDP支持、跳过证书验证、使用规则集



## 2. Clash Verge

以Windows为例

