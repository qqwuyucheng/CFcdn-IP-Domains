GitHub Actions 运行cf2dns_actions
gacjie edited this page on May 3 · 4 revisions
简单介绍
GitHub Actions 运行可解决部分系统环境无法部署的问题。
使用本教程前请先查看文章 CloudFlare SAAS(cname) 接入网站域名 使用SAAS功能接入后再查看本教程操作。

获取 SecretId、SecretKey 对接信息。
腾讯云密钥获取 https://console.cloud.tencent.com/cam/capi
阿里云密钥获取 https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.11.2c6a2fbdh13O53 注意需要添加DNS控制权限 AliyunDNSFullAccess
华为云后台获取 https://support.huaweicloud.com/devg-apisign/api-sign-provide-aksk.html

隐私说明
Fork后的项目无法设置为私有模式。
因此任何人可以公开查看Actions内带有域名的任务日志。
如果不想显示域名，可自行修改代码屏蔽，或者重新部署私有仓库。

Fork项目到自己的仓库
fork.png

配置Repository secrets
进入第二步中Fork的项目，点击Settings->Security->Secrets and variables->Actions，创建CONFIG、DOMAINS、PROVIDER。
actions.png

CONFIG等同于 default_config.json config.json 里面的内容
config配置文件说明.md

DOMAINS等同于 default_domains.json domains.json 里面的内容
domains配置文件说明.md

PROVIDER等同于provider.json 里面的内容

修改run.yml 文件
修改您项目中的 .github/workflows/run.yml 文件，修改定时执行的时长(建议15分钟执行一次)，最后点击 start commit
workflows.png

提交即可在Actions中的build查看到执行情况，如果看到 cf2dns 执行日志中有 CHANGE DNS SUCCESS详情输出，即表示运行成功。需要注意观察下次定时是否能正确运行，有时候GitHub Actions 挺抽风的
all.png build.png
-------------------------------------------------------------------------
config配置文件说明
root edited this page on Jul 7 · 3 revisions
简单介绍
config.json是通用的配置数据文件
default_config.json 为避免更新时覆盖掉配置文件 默认的配置文件

字段说明
{ "type": "v4", //IP类型该字段已废弃
"ipv4": "on", //是否开启IPV4更新，"on"为开启 "off"为关闭 ，开启后更新A记录
"ipv6": "on" , //是否开启IPV6更新，"on"为开启 "off"为关闭 ，开启后更新AAAA记录
"dns_server": 1, //DNS服务商 1为腾讯云dnspod 2为阿里云 3为华为云
"cdn_server": 1, //CDN服务商 1为CloudFlare、2为CloudFront、3为Gcore
"affect_num": 2, //单线路解析IP数量，DNSPOD免费版只支持单线路2个IP，华为阿里可以设置5个
"region_hw": "cn-east-3", //华为云地域，国际版账号需要改为海外地域标识
"region_ali": "cn-hongkong", //阿里云地域，国际版账号需要改为海外地域标识
"ttl": 600, //ttl 建议改为您所用dns支持的最低ttl
"secretid": "AKIDVHfo8CzfjgN", //对接API的 secretid
"secretkey": "ZrVs***gqjOp1zVl", //对接API的 secretkey
"key": "o1zrmHAF", //优选数据服务商提供的KEY
"data_server": 1, //优选数据服务商 1为monitor.gacjie.cn 2为stock.hostmonit.com 3为345673.xyz
"integral": 99999999 //优选数据服务商提供的KEY对应的积分
}

--------------------------------------------------------------------
domains配置文件说明
gacjie edited this page on May 2 · 2 revisions
简单介绍
domains.json是通用的域名数据配置文件
default_domains.json 为避免更新时覆盖掉配置文件 默认的域名配置

字段说明
{ "gacjie.cn": {//域名
"@": ["CM", "CU", "CT"], //主机名：线路[CM=移动 CU=联通 CT=电信]
"www": ["CM", "CU", "CT"],
"tools": ["CM", "CU", "CT"],
"monitor": ["CM", "CU", "CT"]
},
"baota.me": {
"@": ["CM", "CU", "CT"],
"www": ["CM", "CU", "CT"]
}

}
--------------------------------------------------------------------


#### 简单介绍     
本项目基于github.com/ddgth/cf2dns二次开发增加了更多功能与平台支持。    
功能上主要用于自动化将优选IP地址解析到您的域名记录中。    
支持CloudFlare、CloudFront、Gcore优选IPv4&IPv6地址    
支持宝塔面板、python3、GitHub-Actions三种方式部署。    
支持GacJieMonitor(monitor.gacjie.cn)、HostMonit(stock.hostmonit.com)两个平台获取数据。
   
#### 云服务监测平台(https://monitor.gacjie.cn)     
由于近期移动抽风屏蔽eu.org域名，影响监测脚本提交数据，故我把域名全换掉了。     
不再使用eu.org域名，请大家及时更换新的地址链接。    
    
#### 演示图片    
 ![cf2dns.jpg](https://raw.githubusercontent.com/gacjie/cf2dns/main/cf2dns.jpg)   
         
#### 功能说明    
支持从GacJieMonitor(monitor.gacjie.cn)获取CloudFlare、CloudFront、Gcore优选IP地址   
支持从HostMonit(stock.hostmonit.com)获取CloudFlare优选IP地址          
支持从345673.xyz(345673.xyz)获取CloudFlare优选IP地址   
支持将优选IP解析至DNSPOD        
支持将优选IP解析至阿里云解析          
支持将优选IP解析至华为云解析          
支持查询授权余额          
支持ipv4&ipv6同时获取并解析                 
支持宝塔部署cf2dns插件    
支持python3部署运行cf2dns_global    
支持GitHub-Actions-运行cf2dns_actions    
         
#### 宝塔兼容性   
已测试支持以下版本    
aapanel7.0.7   
btpanel7.7.0    
btpanel9.0.0-lts    
         
#### 小广告
[【宝塔】送你10850元礼包](https://www.bt.cn/?invite_code=M19yaHFycXY=)    
[【腾讯云】云产品1折特惠专区](https://curl.qcloud.com/zASK1SLm)     
[【阿里云】云产品爆款特惠](https://www.aliyun.com/minisite/goods?userCode=zqpad1gj)    
         
#### 价格计费    
插件免费提供授权码o1zrmHAF，可永久免费使用。    
[GacJieMonitor](https://github.com/gacjie/cf2dns/wiki/GacJieMonitor付费KEY价格)   
[HostMonit小店](https://shop.hostmonit.com/)   
[345673.xyz](https://345673.xyz/)  
          
### 注意事项     
宝塔安装时请关闭宝塔系统加固插件，会终止安装脚本的执行。     
脚本只会更新电信、移动、联通三网线路的IP，因此还需要将回退源设置到默认线路上。      
使用插件前请确保您的网站域名使用cname或saas方式接入，并且域名解析在dnspod、华为云、阿里云。       
     
#### 使用说明   
[宝塔安装cf2dns插件](https://github.com/gacjie/cf2dns/wiki/宝塔安装cf2dns插件)   
[python3部署运行cf2dns_global](https://github.com/gacjie/cf2dns/wiki/python3部署运行cf2dns_global)  
[GitHub Actions 运行cf2dns_actions](https://github.com/gacjie/cf2dns/wiki/GitHub-Actions-运行cf2dns_actions)  
        
#### 数据备份     
config.json是配置数据    
domains.json是域名数据    
cf2dns插件、cf2dns_global、cf2dns_actions均支持。    
配置完后可以直接备份这俩数据文件，后续需要迁移可直接上传。     
   
#### 2024年07月28日更新记录（V1.7）            
修复已知BUG    
   
#### 常见问题        
      
Q：为什么不支持海外dns解析运营商？     
A：由于cf等cdn属于泛播，移动联通电信需要单独解析，才能实现三网优选。海外dns均不支持国内三网线路解析。      
如不方便使用国内云解析 可以访问 https://monitor.gacjie.cn/page/cname/index.html 获取公共cname地址使用。       
     
Q：使用该插件更新IP后，导致网站全都打不开了？      
A：使用优选IP的前提是，cdn使用cname(别名)方式接入，本插件只是将获取的优选IP解析到您的域名上去，网站打不开是因为你的CDN配置问题。        
     
Q：该插件安全吗？      
A：插件是基于cf2dns增加了宝塔可视化操作界面。并且代码全部公开在github上面，可先自行审查代码再决定是否安装。      
      
Q：为什么不做成其他面板的插件？      
A：由于cf2dns源代码是基于python3编写的，而宝塔面板的运行环境也是python3，所以可以很方便的写成插件，不需要考虑python3环境问题。       
