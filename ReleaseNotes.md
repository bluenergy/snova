_You can download the latest version of the Snova/GSnova [here](http://code.google.com/p/snova/downloads/list)_

Snova/GSnova - Release Notes
### Version 0.22.1 - (2013.06.15) ###
  * Go1.1重新编译以及一些Bug fix

### Version 0.22.0 - (2013.03.23) ###
  * `[C4]`: NodeJS服务端兼容node 0.6
  * `[GAE]`: Java服务端删除对`GoogleAppengine`的api依赖
  * `[GAE]`: Java服务端新的部署工具（无需单独下载Appengine SDK）
  * `[GAE]`: 替换Java的Snappy压缩库
  * `[GAE]`: 修改用户Authorize机制；修改用户密码需要修改服务端部署包中snova.conf重新部署
  * `[ALL]`: Range Fetch机制优化
  * 一些Bug fix以及默认设置的优化

### Version 0.21.0 - (2013.03.09) ###
  * `[C4]`: client&server性能优化（Java&NodeJS）
  * `[C4]`: 增加多任务并发下载支持（默认关闭，需要修改配置`MultiRangeFetchEnable=1`开启）
  * 一些Bug fix以及默认设置的优化

### Version 0.20.3 - (2013.02.20) ###
  * `[C4]`: 服务端优化（Java&C4）
  * `[SPAC]`: 修正firefox通过forward访问某些站点的问题
  * `[SPAC]`: GAE/C4配置多app情况下，支持指定某些站点通过指定app，配置参考[Issue360](http://code.google.com/p/snova/issues/detail?id=360&colspec=ID%20Type%20Status%20Priority%20Modified%20Owner%20Summary)
  * 一些Bug fix以及默认设置的优化

### Version 0.20.0 - (2013.02.05) ###
  * `[C4]`: 支持RC4加密，可替换密钥；默认加密改为RC4加密（Server需要重新部署）
  * `[C4]`: 修改为读写分离的Proxy实现，提高性能
  * `[C4]`: 支持websocket（仅保证在dotcloud和vps上能运行）
  * `[SPAC]`: 提高在SPAC下对google站点的proxy性能
  * `[ALL]`: 移植GSnova(Go)部分特性到Snova(Java)中，配置统一
  * 一些Bug fix以及默认设置的优化

### Version 0.19.4 - (2012.12.16) ###
  * Google站点的连接重试机制优化，对`[GAE], [Google]`有较好的改善
  * 一些Bug fix以及默认设置的优化

### Version 0.19.3 - (2012.12.09) ###
  * GAE/C4/SSH的proxy实现单独绑定无SPAC支持的端口，分别为48101/48102/48103
  * `[C4]`: 支持NodeJS PaaS 平台，优化了Java的Server实现
  * `[C4]`: 支持Https连接到server
  * `[GAE]`: 优化连接管理
  * `[SPAC]`:  cloud\_spac.json & cloud\_hosts.conf强制在启动时更新，用户可编辑user\_spac.json, user\_hosts.conf覆盖cloud配置
  * 一些Bug fix以及默认设置的优化

### Version 0.19.2 - (2012.12.02) ###
  * `[SPAC]`: 修改默认IP库为apnic
  * `[C4]`: 优化HTTP连接管理
  * 一些Bug fix以及默认设置的优化

### Version 0.19.1 - (2012.11.24) ###
  * `[C4]`: 快速关闭主动断开的连接
  * 默认设置的优化

### Version 0.19.0 - (2012.11.23) ###
  * `[C4]`: 重构，性能提高两倍以上(Server需要重新部署）
  * 一些Bug fix以及默认设置的优化

### Version 0.18.4 - (2012.11.17) ###
  * SPAC规则优化（增加Range属性，修改默认规则）
  * 一些Bug fix

### Version 0.18.3 - (2012.11.10) ###
  * 通过可信DNS解析Google站点
  * DNS默认开启缓存，仅解析IPv4，提升DNS性能
  * exe文件增加icon
  * C4支持域名+路径配置
  * 修改默认youtube路由规则（优先直接访问）
  * 一些Bug fix

### Version 0.18.0 - (2012.11.03) ###
  * 默认端口48100增加socks协议支持
  * DNS模块改进以及性能提升
  * 一些Bug fix

### Version 0.17.2 - (2012.10.23) ###
  * `[GAE]`: 升级snappy库，解决之前的snappy传输解压缩问题
  * `[GAE]`: 修改默认的`LocalProxy为GoogleHttps`
  * `[GAE]`: Go server自带一个deployer，简化部署
  * `[SPAC]`: 将spac.json分为用户自定义部分和网络同步部分
  * `[Hosts]`: 将hosts.conf分为用户自定义部分和网络同步部分


### Version 0.17.0 - (2012.10.21) ###
  * 仅gsnova更新
  * 增加简单的本地Web UI
  * `[SSH]`: 增加SSH支持，可配置多个服务端；支持用户名/密码以及用户名/密钥文件两种鉴权方式（密钥支持RSA/DSA）
  * `[GAE]`: 支持通过Web UI share/unshare appid
  * `[SPAC]`: 内置支持GFWList解析，解释，以及spac filter（Filter: IsBlockedByGFW）
  * `[SPAC]`: 内置wipmania IP库，以及spac filter（Filter: IsHostInCN）
  * `[SPAC]`: spac.json修改后自动生效
  * `[SPAC]`: 支持远程同步spac.json规则脚本，默认关闭
  * `[SPAC]`: 支持自定义的生成PAC文件中的proxy地址（配置`[SPAC]`下的PACProxy）
  * 性能优化，减少内存占用

### Version 0.16.0 - (2012.10.06) ###
  * 仅gsnova，c4服务端更新
  * `[Hosts]`:支持直接IP转发
  * `[Hosts]`:集成smarthosts服务（此服务有些小问题，某些IP需要去除 `[Hosts]->ExceptHosts`）
  * `[Hosts]`:强制指定的sites重定向至https（`[Hosts]->RedirectHttps`）
  * `[Hosts]`:TCP查询可信DNS服务代替系统DNS
  * `[Hosts]`:集成一个远程https DNS查询服务，在TCP查询可信DNS服务失效时生效
  * `[Hosts]`:HTTP CRLF注入，可直接访问某些站点（`[Hosts]->InjectCRLF`）
  * `[Hosts]`:多并发Range访问；类似GAE中多并发访问视频站点的实现（`[Hosts]->InjectRange`）
  * `[SPAC]`:增强的规则实现，一般某个Proxy失败，自动切换至下一个实现
  * `[SPAC]`:一个根据gfwlist以及自定义规则自动生成pac的实现（编辑user-gfwlist.txt）
  * `[SPAC]`:一个生成的pac的web访问接口（`http://127.0.0.1:48100/pac/gfwlist`）
  * `[GAE]`:内存中缓存生成的伪造证书，降低通过GAE访问https站点时的CPU
  * `[C4]`:删除rsocket相关code
  * `[C4]`:服务端集成httpdns（`https://github.com/yinqiwen/httpdns`）

### Version 0.15.0 - (2012.08.18) ###
  * `[GSnova]`：golang实现snova GAE client，支持snova大部分功能，包括GAE/SPAC/C4
  * `[C4]`：重构原实现（client&server），增强性能，server兼容Go/Java client
  * `[SPAC]`：修复socks forward失败问题

### Version 0.15.0(beta) - (2012.07.18) ###
  * `[GSnova]`：golang实现snova GAE client

### Version 0.14.0 - (2012.06.16) ###
  * `[C4]`：完善RSocket模式，增加upnp支持
  * `[GAE]`：下载大文件/视频时的并发bug修复（某些情况下看youtube视频会触发）
  * `[GAE]`：Java Server的bug（某些情况如twitter发推会触发）
  * `[GAE]`：Go Server执行blacklist命令crash的问题

### Version 0.13.0 - (2012.04.22) ###
  * `[C4]`：[试验性]增加RSocket模式，增强C4模式通信的实时性
  * `[GAE]`：GUI修改配置文件bug

### Version 0.12.0204 - (2012.01.21--2012.02.04) ###
  * `[C4]`：性能优化：增加HTTP连接模拟全双工通信机制的实现，提高性能；需要重新部署server
  * `[GAE]`：GUI优化——提高启动速度；优化体验
  * `[ALL]`：所有配置文件改为纯text格式，支持修改后自动载入

### Version 0.12.0120 - (2012.01.08--2012.01.20) ###
  * 新的plugin: C4, 代替原来的Heroku，支持更多的PaaS平台`Heroku/CloudFoundry/OpenShift/Jelastic`
  * `[GAE]`:Range fetch时增加错误retry机制
  * `[SPAC]`:两个新的内置函数 `IsBlockedByGFW/InHosts`

### Version 0.12.0107 - (2012.01.01--2012.01.07) ###
  * `[GAE]`:XMPP大消息（>4KB）情况下的bug fix（Client & Java Server）
  * `[GAE]`:完全异步化的HTTP连接（Client）

### Version 0.12.0101 - (2011.12.25--2012.01.01) ###
  * 新的plugin: SPAC, 增强的PAC功能.
  * 新的plugin: Heroku, 类似于GAE，需要部署server到Heroku云平台.
  * framewok/gae/spac支持配置修改自动加载
  * 伪造的证书文件持久化到硬盘

### Version 0.11.1111 - (2011.11.11--2011.12.18) ###
  * Go/Java实现的GAE server端.
  * Snappy压缩支持.
  * 插件体系.
  * 用户鉴权支持.
  * AppID共享.
  * 简单的GUI shell.