Openwrt的翻墙解决方案
===========

# 使用方法
从 [gfw](gfw) 下载预编译好的ipk包(目前 fastdns 还需要手工编译)上传到路由器上并执行 opkg update 和 opkg install 安装，如果有兴趣重新编译的话可以参见: [UsePackage](UsePackage.md)  

* fastdns: 使用VPN避免DNS污染，同时还能使用本地CDN: [AntiDNSPoisoning](AntiDNSPoisoning.md)
* gfw-vpn: 通过VPN翻墙的同时又能使用本地线路访问国内网络: [PPTPVPN](PPTPVPN.md)
* gfw-dualpptp: 使用双线VPN翻墙，即使其中一条线路断线也能正常翻墙，同时还能降低丢包率: [DualLinePPTPVPN](DualLinePPTPVPN.md)
* 优化内核pptp性能，同时使其支持缓存数据包功能: [OptimizePPTPVPN](OptimizePPTPVPN.md)


# 关于为何写该系列
我理想中的翻墙方案应该是对用户完全透明，让上网体验就像墙根本不存在一样，只需设置一次，其后不需要任何维护。其次是不能降低安全性，不能因为翻墙而引入新的安全隐患。最后还需要有全平台多设备的支持。但是看目前各翻墙方法都或多或少有些问题，比如：
* Host翻墙：没法上被封ip的网站，需要手动更新，或者等发布者更新，而且更新都是事后的。有安全隐患，发布者可以恶意（或者因为账号被黑）放钓鱼ip在其中。在手机上设置并不容易。
* SSH代理：有tcp over tcp的性能问题，如果软件不支持代理就比较麻烦，需要经常更新代理规则。
* 各种其他代理：自由门、无界之类不开源的都有安全隐患，开源的像goagent会引入中间人攻击隐患，例如2013年初对GitHub的中间人攻击，如果不翻墙的话就会看到浏览器警告，而用goagent使用国内的gae就不会有任何警告，另外同样需要经常更新代理规则。
* 各种VPN：虽然可以通过修改路由表区分国内国外流量，但是没法区分p2p和上网流量，如果挂bt或者emule会有大量流量走VPN。另外虽然可以通过autoddvpn提供DNS CDN功能，但是需要维护域名列表。

而本系列解决了:
* DNS CDN的问题，而且不需要维护域名列表，虽然我天天都在用GitHub，但是Github被域名污染我是看了新闻才知道的。整个解决方案只需一次设置，其后就可以不管了。
* 没有引入新的安全问题(fastdns除外)，虽然PPTP不安全，但是提供了至少和不翻墙一样的安全程度，HTTP依然是HTTP，HTTPS依然是HTTPS，也不需要在翻墙终端上装任何软件或进行任何设置。
* 全平台多设备支持，因为是设置在路由器上的，所以对路由器后的设备完全透明。
* 基本没有性能损耗，和正常上网的平均（注1）速度一样。
* 虽然用到了中国地区的ip段，但是鉴于ipv4地址也分配的差不多了，这个数据的更新并不频繁，这次更新本系列做了个最新的cn.zone，和我两年前生成的只有20多行不一样。另外即使不更新，也不会撞墙。
* 可以在路由器上安装VPN服务器，这样只要在其他地方拨路由器的VPN即可翻墙

注1：不翻墙访问国外网站会遇到有的卡，有的不卡，而用VPN则是要么全部都卡，要么全部都不卡，所以这里说平均。另外用双线VPN可以以本地带宽减半为代价使得访问所有网站都不卡。  
注2：本来想将文章发到国内相关论坛，发现不是邀请注册就是各种发贴限制。其中一个论坛好不容易等了一小时过了见习期，结果要发贴必须上传头像，而头像只能通过flash上传。GFW就是为了阻挡信息传播而生，而这么折腾用户在挡住spam的同时又阻挡了多少信息的传播呢？
