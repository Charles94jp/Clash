<h1 style="text-align: center;">OpenClash & Clash Verge Configuration</h1>

自用clash配置，适用于 [clash_meta](https://github.com/MetaCubeX/mihomo) 内核

包括openwrt([immortalwrt](https://github.com/immortalwrt/immortalwrt)) 上的 [OpenClash](https://github.com/vernesong/OpenClash) 和 终端 上的 [clash-verge-rev](https://github.com/clash-verge-rev/clash-verge-rev)

<br></br>

## 1. Clash Verge

以Windows为例

1. 打开订阅页面，双击`全局扩展覆写配置`，粘贴

```yaml
#自定义 geodata url
geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.metadb"

geodata-mode: true # 使用geoip.dat代替Country.mmdb
geo-auto-update: true # 是否自动更新 geodata
geo-update-interval: 24 # 更新间隔，单位：小时
```

2. 设置，单击更新GeoData
3. 将机场订阅url编码后插入以下url

```
https://api.dler.io/sub?target=clash&url=【url编码后的机场订阅地址】&config=https://raw.githubusercontent.com/Charles94jp/Clash/master/Custom_Clash.ini
```



## 2.OpenClash

### 2.1 概述与准备

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

### 2.2 订阅转换

编辑订阅

在线订阅转换

服务器api.asailor.org

模板拉到最下方选自定义

```
https://testingcf.jsdelivr.net/gh/Charles94jp/Clash@master/Custom_Clash.ini
```

启用：UDP支持、跳过证书验证、使用规则集





