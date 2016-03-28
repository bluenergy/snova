# Step 1: 准备依赖环境 #
  * 运行部署snova需要JRE/JDK 1.6+
  * 运行部署gsnova无需任何依赖

# Step 2: 创建`GoogleAppEngine帐号/应用` #
> 在官方站点创建自己的GAE帐号以及appid http://appengine.google.com/

# Step 3: 准备GAE SDK环境用于部署上传(可选，Go版本自带一个部署工具) #
  * 下载解压Google App Engine SDK(Java/Go) ([最新版本](http://code.google.com/intl/en/appengine/downloads.html))， snova-gae支持Java/Go两种语言的server端实现，任选一个即可

# Step 4: 部署Server到Appengine服务器 #
  * Go版本(和Java版本任选其一)
> > 两种方式
    * 用自带的Deployer部署
      * 下载并解压`snova-gae-golang-server-[version].zip`
      * windows用户执行deployer.exe；Mac/Linux用户执行python deployer.py
      * 按照deployer的指示输入，执行部署
    * 用`Appengine Go SDK`部署
      * 下载并解压`snova-gae-golang-server-[version].zip`
      * 进入解压的目录, 修改app.yaml， 将`application: snova-master`中snova-master值改为自己创建的appid
      * 执行appcfg.py update `snova-gae-gserver-<version>`上传(appcfg.py在'<Google App Engine Go SDK>/'下
  * Java版本
> > 两种方式
    * 用自带的Deployer部署
      * 下载并解压`snova-gae-java-server-[version].zip`
      * windows用户执行deployer.bat；Mac/Linux用户执行deployer.sh
      * 按照deployer的指示输入，执行部署
    * 命令行方式
      * 下载并解压`snova-gae-java-server-[version].zip`
      * 进入解压的目录, 修改war/WEB-INF/appengine-web.xml， 将`<application>`值改为自己创建的appid
      * 执行appcfg.cmd/appcfg.sh update war上传, 注意在解压后进入的目录执行(appcfg在'<Google App Engine Java SDK>/bin'下 )


# Step 5: 配置客户端 #
Client可选gsnova(Go)或者snova(Java).
## gsnova(Go) ##
  * 修改gsnova.conf
> > 目前通过修改配置文件gsnova.conf实现,具体说明可参考snova(Java)部分，eg:
```
      [GAE]
      Enable=1
      WorkerNode[0]=myappid1
      WorkerNode[1]=myappid2
```
  * HTTPS站点代理需要导入伪造的证书。伪造证书为cert/Fake-ACRoot-Certificate.cer
  * 如果不配置appid的话，client会到master node上获取数个共享的appid用于自身启动，注意—— 共享的appid只能用于匿名用户使用（匿名用户的概念看这里[Authorization](http://code.google.com/p/hyk-proxy/wiki/Authorization)）
  * 不支持XMPP
## snova(Java) ##
  * 修改snova.conf
> > snova.conf和gsnova.conf格式一致，修改过程可参考'修改gsnova.conf'一节