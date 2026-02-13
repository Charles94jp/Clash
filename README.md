<h1 style="text-align: center;">Clash配置与订阅转换</h1>

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

<br></br>

## 2.OpenClash

### 2.1 概述与准备

[OpenClash配置示例及订阅转换模板，无需套娃，无泄漏](https://github.com/Aethersailor/Custom_OpenClash_Rules)

先参照上述仓库wiki进行配置

注意先后顺序，推荐先设置Geo相关的数据，趁你的网络还能访问GitHub，否则若按照wiki中设置，重启后可能无法成功，因为没先下载Geo相关的数据。

先更新clash客户端和内核到最新发行版

1. GEO 数据库订阅，全点一遍检查并更新，都会重启一次clash
1. 覆写-meta-启用 GeoIP Dat 版数据库，勾选后应用，会重启clash下载这个dat数据

完成这两个步骤再按wiki从头设置。



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



<br></br>

## 3. 特别的

**👉内核流量：**

1. openclash开启`实验性：绕过指定区域 IP`绕过中国大陆后，国内流量不会过核心，在zashboard连接中就看不到了

2. clash verge系统代理在Windows代理设置处设置了局域网IP不代理，我们也可以追加一些内外域名到其中，可以在clash上设置。这些流量不会经过内核，而中国大陆域名是会过核心的，会命中大陆直连的规则。



**👉cdn加速：**

加速github

1. Clash Verge不需要，因为订阅转换服务器是`api.dler.io`，也可以改用`api.asailor.org`，由它们去请求GitHub
2. openclash需要用到cdn加速，确保没开启服务时能更新。

值得注意的是cdn通常n~24小时更新一次，想立马更新就换域名：testingcf.jsdelivr.net、fastly.jsdelivr.net



**👉重复代理问题：**

手机圈x、Loon、surge连接家里openclash wifi会不会重复代理？

正常情况不会，因为目前机场用的是专线，需要先在国内访问入口服务器，再通过专线到国外出口服务器，再去目标网站服务器。openclash看来你就是去往国内入口服务器。

不正常的情况也可能发生

原因：机场提供了回国节点，意思是从国外穿回国内，是反的，大陆不能使用

openclash模式下，手机的代理app Loon通过url-test发现回国节点延迟最低，选了它，而它必须先直连入口服务器，而回国节点入口服务器在海外，直接访问，因为有openclash，成功了

流量：loon（去回国节点入口） - openclash（代理）- 国内入口 - 国外出口 - 回国节点入口

到这一步，如果继续回国，就会导致无法访问spotify的

但是，如果这个回国节点的入口聪明的话，就不会发到国内，而是直接访问spotify，就访问通了。这里就多了一次过openclash内核代理，过回国节点入口

解决办法：loon排除掉回国节点、非大陆使用节点。openclash自定义规则rules中指定你机场的入口为直连，打开订阅文件就能看到机场的入口地址
