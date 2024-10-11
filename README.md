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
    
#### 演示图片    
 ![cf2dns.jpg](https://raw.githubusercontent.com/gacjie/cf2dns/main/cf2dns.jpg)   
        
#### 公告通知    
出于运维成本、长久稳定性等情况考虑。    
目前平台域名更换为WeTest.vip。    
请各位用户及时将插件更新到1.9版本以上。     
    
#### 接口支持    
CloudFlare官方优选(WeTest.vip)   更新频率15IP/15分钟   
CloudFlare官方优选(HostMonit.com)更新频率15IP/15分钟   
CloudFlare官方优选(345673.xyz)   更新频率15IP/15分钟    
CloudFront官方优选(WeTest.vip)   更新频率15IP/15分钟   
Gcore官方优选     (WeTest.vip)   更新频率15IP/15分钟   
        
#### 解析支持    
[华为云解析](https://support.huaweicloud.com/devg-apisign/api-sign-provide-aksk.html)   
[阿里云解析](https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.11.2c6a2fbdh13O53)   
[腾讯云解析(DNSPOD)](https://console.cloud.tencent.com/cam/capi)   
         
#### 宝塔兼容性   
已测试支持以下版本    
aapanel7.0.7   
btpanel7.7.0    
btpanel9.0.0-lts    
         
#### 小广告
   
[【弘速云hosuyun.com】香港、美国高性能优质线路服务器，新用户首购五折特惠。](https://www.hosuyun.com/)  
[【宝塔bt.cn】宝塔产品特惠，linux专业版1年仅需￥699。](https://www.bt.cn/p/2PcEKn)    
[【腾讯云cloud.tencent.com】云产品1折特惠，2核2G4M仅需108元/年](https://curl.qcloud.com/zASK1SLm)     
[【阿里云aliyun.com】云产品爆款特惠，2核2G3M仅需82元/年](https://www.aliyun.com/minisite/goods?userCode=zqpad1gj)    
         
#### 价格计费    
插件免费提供授权码o1zrmHAF，可永久免费使用。    
[WeTest.vip付费服务说明](https://github.com/gacjie/cf2dns/wiki/WeTest付费服务说明)   
[WeTest.vip付费授权码购买](https://www.wetest.vip/dash/Account/register)   
[HostMonit.com付费授权码](https://shop.hostmonit.com/)   
[345673.xyz付费授权码](https://345673.xyz/)  
          
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
   
#### 2024年09月30日更新记录（V1.9）            
更新接口地址为WeTest.vip   
插件版增加对传入的字符串过滤空格、换行符   
   
#### 常见问题        
   
Q：为啥别人使用优选很快，我使用优选访问慢？      
A: 通常优选系统只会测用户端 - CDN节点的速度，但是节点也是要访问源站获取数据，源站与节点链接不稳定也会导致整体访问慢。   
A：建议增加缓存或有条件更换国际线路较好的源站服务器来优化链接速度。   
      
Q：为什么不支持反代优选？      
A：本项目是为了建站而开发，反代优选IP为扫描的第三方的服务器，存在不可控的安全隐患。   
A：目前已有因使用反代优选导致域名被注册机构禁用的先例。      
A：因此本项目未来也不会提供反代优选，除非您自行添加相关接口。       
         
Q：为什么不支持海外dns解析运营商？        
A：由于cf等cdn属于泛播，移动联通电信需要单独解析，才能实现三网优选。海外dns均不支持国内三网线路解析。      
A：如不方便使用国内云解析 可以访问 https://www.WeTest.vip 获取公共cname地址使用。       
     
Q：该插件安全吗？      
A：插件是基于cf2dns增加了宝塔可视化操作界面。并且代码全部公开在github上面，可先自行审查代码再决定是否安装。      
      
Q：为什么不做成其他面板的插件？      
A：由于cf2dns源代码是基于python3编写的，而宝塔面板的运行环境也是python3，所以可以很方便的写成插件，不需要考虑python3环境问题。       
