
# 简要说明 #
  * 部署过程一般是将C4的服务端在vps上启动
  * 根据自己的需要任意选择一个Java/NodeJS实现部署，部署过程参考下个章节
  * 部署服务端成功后，需要修改snova/gsnova的客户端配置


# 部署服务端 #
## Java ##
### Step 1: 准备Java环境 ###
  * Java要求1.6以上

### Step 2: 部署启动 ###
  * 下载`snova-c4-server-[version].war`， 放在任意地方
  * 下载`JettyRunner`[下载地址](http://repo2.maven.org/maven2/org/mortbay/jetty/jetty-runner/8.1.9.v20130131/jetty-runner-8.1.9.v20130131.jar)，下载的文件放到war文件同一目录
  * 在命令行下进入war文件所在目录，执行下面命令启动
```
       java -jar jetty-runner-8.1.9.v20130131.jar snova-c4-server-[version].war
```

  * 注意默认绑定端口为8080，需要绑定其它端口参考jetty runner帮助；同时记住VPS的外网IP


---


## NodeJS ##
### Step 1: 注册`NodeJS`环境 ###
  * 到官方链接[NodeJS](http://nodejs.org/)下载安装NodeJS，注意要求0.6以上

### Step 2: 部署启动 ###
  * 将`snova-c4-nodejs-server-[version].zip`解压缩到任意的空目录下
  * 在命令行下进入war文件所在目录，执行下面命令启动
```
      node server.js
```
  * 注意默认绑定端口为8080，需要绑定其它端口需要修改server.js；同时记住VPS的外网IP


---


# 修改客户端配置 #
客户端可选gsnova(Go)或者snova(Java)
## gsnova(Go) ##
  * 修改gsnova.conf中C4部分，填入VPS IP:端口，重启gsnova生效
```
     [C4]
     #Enable改为1，C4才能生效，默认为0关闭
     Enable=1
     #修改domain为Step2创建的域名, 可加多个
     WorkerNode[0]=10.101.10.10:8080
     WorkerNode[1]=10.101.10.11:8080
```
  * 修改gsnova.conf中SPAC下默认的Proxy实现为C4
```
     [SPAC]
     Enable=1
     #默认Proxy实现，初始为GAE
     Default=C4
```
## snova(Java) ##
  * 从snova-0.12.0120开始自动集成c4 plugin，无需单独安装
  * 修改`<snova>/plugins/c4/conf/c4-client.conf`
```
       [C4]
      #修改填入VPS IP:端口
       WorkerNode[0]=10.101.10.10:8080
       WorkerNode[1]=10.101.10.11:8080
```
  * 修改`<snova>/conf/snova.conf`，将C4改为默认proxy实现(可选)
```
      [Framework]
      .........
      ##Can choose in plugins, default is GAE
      ProxyService=C4
```
## SPAC(可选) ##
  * gsnova用户参考SPAC的说明[SpecialProxyAutoConfigOnGSnova](SpecialProxyAutoConfigOnGSnova.md)修改json配置打造自己的proxy环境
  * snova用户参考SPAC的说明[SpecialProxyAutoConfig](http://code.google.com/p/snova/wiki/SpecialProxyAutoConfig)修改`SelectProxy`方法打造自己的proxy环境

