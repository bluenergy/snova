

# 常见问题 #
> 以下列出一些snova/gsnova的常见问题，若有其它疑问，请到[Snova讨论组](https://groups.google.com/forum/?fromgroups#!forum/hyk-proxy)中提问。
> ## snova/gsnova是什么？snova和gsnova有何区别？ ##
    * snova是一个基于各种PaaS平台打造自用Proxy，兼顾一些反干扰的工程实现的项目。
    * snova主要分为两部分：Client和Server。只有两者结合才能正常工作。
    * Client的实现有两个分支：Snova(Java实现)和GSnova(Go实现)，两者在实现上有较大的差别，不过基本代理功能近似相同。
    * Server的实现则根据PaaS平台的不同，支持三类PaaS：Google Appengine,Java PaaS和NodeJS PaaS.
> ## 什么是PaaS？GAE, Java PaaS和NodeJS Paas有何区别？ ##
    * 关于PaaS请参考wiki的解释[平台即服务](http://zh.wikipedia.org/wiki/%E5%B9%B3%E5%8F%B0%E5%8D%B3%E6%9C%8D%E5%8A%A1)。
    * GAE（Google Appengine）就是一种典型的PaaS，不过由于GAE提供的是私有非标准API，所以这里单独将其与Java/NodeJS PaaS区分开来。
    * Java PaaS这里指的是一类支持[Java War包](http://en.wikipedia.org/wiki/WAR_file_format_(Sun))部署的PaaS， 目前提供服务的有Heroku/openshift/Cloudfoundry/Dotcloud等
    * NodeJS PaaS这里指的是支持部署NodeJS代码的PaaS， 目前提供服务的有Heroku/openshift/Cloudfoundry/Dotcloud/Appfog/Modulus等
> ## 设置代理为127.0.0.1:48100后, 为何有些站点是直接访问的？比如通过查ip的网站，发现设置代理后还是显示“某某电信”。 ##
    * 默认gsnova在SPAC开启情况下，有GFWList过滤，部分站点是直连访问的
    * 可以参考[Issue 272](http://code.google.com/p/snova/issues/detail?id=272&colspec=ID%20Type%20Status%20Priority%20Modified%20Owner%20Summary)关闭SPAC支持
> ## 什么是C4？和GAE有何区别？ ##
    * C4是snova中支持Java/NodeJS PaaS的代理实现的名字。初始含义为Cloud4, 不过目前已经支持更多的PaaS了。当然现在也可以解释为这个[C4](http://en.wikipedia.org/wiki/C-4_(explosive))。
    * 基于C4的代理实现和GAE的实现有较大区别。最直观的区别是可以实现非中间人劫持方式的HTTPS代理，以及下载大文件时的良好支持。
> ## 如何在只能用socks代理的场景下使用snova作为代理？ ##
    * 目前只有gsnova在端口上同时支持socks/http协议，可以直接用gsnova的监听IP:端口设置为socks代理
    * 注意默认只支持HTTP流量，一些UDP/非HTTP的TCP流量是无法通过gsnova的
> ## gsnova启动时会自动打开一个网页，如何关闭这个动作？ ##
    * 把gsnova.conf中AutoOpenWebUI的开头#注释去掉
> ## 部署C4到Heroku/Cloudfoundry/Openshift等PaaS时遇到错误了，如何处理？ ##
    * 首先仔细看下wiki，看看是否是哪一步执行错误。
    * 其次到这些PaaS的官方网站上搜索这类错误的解决方法。一般都能找到答案。
    * 最后强调一下，这类PaaS的部署对于一般用户来说是相当困难的，大部分是全程需要命令行操作。
> ## 如何修改C4的默认RC4密钥？ ##
    * 首先将服务端的压缩包（Java的war文件或者nodejs的zip文件）放到gsnova的安装目录下（和gsnova.exe同一目录）
    * 启动gsnova后，在gsnova的web界面中选择Config/C4 (http://127.0.0.1:48100/setting.html?target=c4), 点击Generate按钮，则会生成一个用密钥作为文件名前缀的新压缩文件；
    * 用生成的文件用同样的部署方法部署
    * 修改gsnova的gsnova.conf（snova用户修改snova,conf）,将`[Misc]`下的RC4Key修改为生成的key
> ## 如何切换C4使用https或者websocket协议？ ##
    * Https: 将`WorkerNode[0]=xx.xx.com改为WorkerNode[0]=https://xx.xx.com`
    * Websocket: 将`WorkerNode[0]=xx.xx.com改为WorkerNode[0]=ws://xx.xx.com`