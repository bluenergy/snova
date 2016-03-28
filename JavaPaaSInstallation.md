
# 简要说明 #
  * 部署过程一般是将C4的服务端实现war包部署到PaaS平台上,这个过程每个PaaS平台都不一样
  * 根据自己的需要任意选择一个PaaS平台部署，部署过程参考下个章节
  * 部署服务端成功后，需要修改snova/gsnova的客户端配置
  * 以下是几个Java PaaS平台的简要部署指南以及snova/gsnova的客户端配置修改指南

# 部署服务端 #
## Heroku ##
### Step 1: 准备Heroku Client环境 ###
按照Heroku官方的[QuickStart](http://devcenter.heroku.com/articles/quickstart)注册帐号，安装Heroku Toolbelt（注册较简单，仅需要email）

### Step 2: 部署Snova C4的服务端到Heroku ###
  * 下载`snova-c4-server-[version].war`， 放在任意地方
  * 在命令行下war文件所在目录，依次顺序执行以下的命令,每一行单独执行
```
       heroku login
       heroku plugins:install https://github.com/heroku/heroku-deploy  --只需执行一次，以后不用执行
       heroku apps:create      --此步创建一个app，名字随机，记住此步的appname（域名不含.herokuapp.com）。更新不用执行此步
       heroku deploy:war --war <path_to_war_file> --app <app_name> 
```
  * 留意执行“heroku apps:create”时的输出，一般会显示创建的域名，为 "xx.herokuapp.com", 记下该域名，为配置Client准备（执行heroku apps可列出所有app）


---


## Openshift ##
### Step 1: 注册`OpenShift`环境, 安装`OpenShift`部署工具 ###
到官方链接[OpenShift注册](https://openshift.redhat.com)注册帐号,参考说明[rhc安装](https://openshift.redhat.com/community/developers/rhc-client-tools-install)安装部署工具

### Step 2: 部署服务到`OpenShift` ###
  * 将`snova-c4-server-[version].war`放到任意的空目录下，然后在命令行下进入该目录，逐个执行下面的命令
```
rhc setup        --登录
rhc-create-domain -n <domainName>  --创建主域名， 部署新应用是这一步可不执行
rhc-create-app -a <appname> -t jbossews-1.0  --创建app,执行后会在当前目录下创建一个appname的目录名；注意保留此目录用于以后更新

cd <appname>
mv snova-c4-server-[version].war webapps/ROOT.war
--上一步将snova-c4-server-[version].war改名为ROOT.war, 移动到webapps目录下， windows用户参考说明操作 

git rm -r src pom.xml   此步重新部署同一个app时可不执行
git add .
git commit –m “deploy”
git push

```
  * 浏览器中输入`<appname>-<domainName>.rhcloud.com`， 查看是否部署成功，否则检查上述步骤是否执行成功


---

## `CloudFoundry` ##
### Step 1: 注册`CloudFoundry`环境，安装`CloudFoundry`部署工具 ###
  * 到官方链接[CloudFoundry](https://my.cloudfoundry.com/signup)注册帐号， 注意，注册不是马上成功，一般第二天才会收到注册成功的邮件，其中包含用户名密码
  * 参考官方说明安装命令行工具vmc，注意安装依赖ruby以及gem的安装。   [vmc安装](http://start.cloudfoundry.com/tools/vmc/installing-vmc.html)

### Step 2: 部署服务到`CloundFoundry` ###
  * 将`snova-c4-server-[version].war`放到任意的空目录下，然后在命令行下进入该目录，逐个执行下面的三行命令。
```
vmc target api.cloudfoundry.com
vmc login
vmc push <appnamexxx>  ——执行此处appnamexxx为任意名称，为域名一部分，此命令执行后有类似下面的交互内容，参照下面的输入选择；更新app亦执行此命令。

user@ubuntu:~/snova-deploy$ vmc push appnamexxx
Instances> 1

1: java_web
2: other
Framework> 1       

1: java
2: java7
3: other
Runtime> 1

1: 64M
2: 128M
3: 256M
4: 512M
Memory Limit> 512M

Creating appnamexxx... OK

1: appnamexxx.cloudfoundry.com
2: none
URL> appnamexxx.cloudfoundry.com

Updating appnamexxx... OK

Create services for application?> n

Bind other services to application?> n

Save configuration?> n

Uploading appnamexxx... OK
Starting appnamexxx... OK
Checking appnamexxx... OK

```
  * 浏览器中输入`<appnamexxx>.cloudfoundry.com`， 查看是否部署成功，否则检查上述步骤是否执行成功


---


## Dotcloud ##
### Step 1: 注册Dotcloud帐号，准备Dotcloud Client环境 ###
  * 按照Dotcloud官方的[Dotcloud](https://www.dotcloud.com)注册帐号，遵循安装说明[Installing the CLI](http://docs.dotcloud.com/0.9/firststeps/install/)安装Dotcloud Client
  * 安装完后，命令行下执行以下命令初始化dotcloud client
```
       dotcloud setup
```

### Step 2: 部署Snova C4的服务端到Dotcloud ###
  * 下载`snova-c4-server-[version].war`， 改名为ROOT.war,放在任意空目录中
  * 下载[dotcloud.yml](http://snova.googlecode.com/svn/trunk/repository/dotcloud.yml), 放到和ROOT.war同一个目录下
  * 在命令行下进入war文件所在目录，依次顺序执行以下的命令,每一行单独执行
```
        dotcloud create <app_name>     --创建app，只需执行一次
        dotcloud push                  --上传app，注意失败后重试一次
```
  * 如果是更新C4服务端，注意保留上步的目录，替换ROOT.war后执行dotcloud push即可
  * 留意执行“dotcloud push”时的输出，一般会显示创建的域名，为 "xx.dotcloud.com", 记下该域名，为配置Client准备（执行heroku list可列出所有app）


---


## Appfog ##
### Step 1: 准备Appfog账号以及安装Client环境 ###
  * 到[Appfog](http://www.appfog.com/)注册账号，并按照官方的[AF Command Line Tool](https://docs.appfog.com/getting-started/af-cli)说明下载安装appfog的命令行工具。
  * 注意：Appfog部署C4后的效果较差，问题不少，愿意研究的可以尝试

### Step 2: 部署Snova C4的服务端到Appfog ###
  * 下载`snova-c4-server-[version].war`， 放在任意地方
  * 在命令行下进入war文件所在目录，依次顺序执行以下的命令,每一行单独执行
```
       af login
       af push <appname> --path ./snova-c4-server-0.19.0.war  
```
  * 执行af push命令后会出现下面输出，参考下面输入选择（注意， 以下实际appname用xxx代替， 内存可选择更高，以提高部署的app性能，下面选择512M仅为示范）
```
       Detected a Java Web Application, is this correct? [Yn]: Y
      1: AWS US East - Virginia
      2: AWS EU West - Ireland
      3: AWS Asia SE - Singapore
      4: Rackspace AZ 1 - Dallas
      5: HP AZ 2 - Las Vegas
      Select Infrastructure: 3
      Application Deployed URL [xxx.ap01.aws.af.cm]: 
      Memory reservation (128M, 256M, 512M, 1G, 2G) [512M]: 512M
      How many instances? [1]: 
      Create services to bind to 'snova'? [yN]: n
      Would you like to save this configuration? [yN]: n
      Creating Application: OK
       Uploading Application:
       Checking for available resources: OK
       Processing resources: OK
       Packing application: OK
       Uploading (3K): OK   
       Push Status: OK
       Staging Application 'xxx': OK                                                 
       Starting Application 'xxx': OK 
```
  * 留意执行“af push”时的输出，出现 Application Deployed URL的一行中含部署后的app的域名，记住次域名，用于配置C4。例如：
```
        Application Deployed URL [xxx.ap01.aws.af.cm]: 
        其中xxx.ap01.aws.af.cm即为域名
```


---


## `Jelastic` ##
### Step 1: 注册`Jelastic`环境 ###
  * 到官方链接[jelastic](http://jelastic.com)注册帐号
  * 注意jelastic的免费期较短，有意尝试的可以部署

### Step 2: 部署 ###
  * 完全图形化的操作，无需安装工具，按照说明将`snova-c4-server-[version].war`上传并deploy到ROOT下即可   [jelastic部署指南](http://jelastic.com/docs/upload-deploy-application)


---


# 修改客户端配置 #
客户端可选gsnova(Go)或者snova(Java)
## gsnova(Go) ##
  * 修改gsnova.conf中C4部分，填入之前创建的域名，重启gsnova生效
```
     [C4]
     #Enable改为1，C4才能生效，默认为0关闭
     Enable=1
     #修改domain为Step2创建的域名, 可加多个域名
     WorkerNode[0]=xyz.herokuapp.com
     WorkerNode[1]=xyz.cloudfoundry.com
```
  * 修改gsnova.conf中SPAC下默认的Proxy实现为C4
```
     [SPAC]
     Enable=1
     #默认Proxy实现，初始为GAE
     Default=C4
```
## snova(Java) ##
  * 修改`<snova>/conf/snova.conf`，修改方法参考gsnova以上部分
## SPAC(可选) ##
  * gsnova用户参考SPAC的说明[SpecialProxyAutoConfigOnGSnova](SpecialProxyAutoConfigOnGSnova.md)修改json配置打造自己的proxy环境
  * snova用户参考SPAC的说明[SpecialProxyAutoConfig](http://code.google.com/p/snova/wiki/SpecialProxyAutoConfig)修改`SelectProxy`方法打造自己的proxy环境

