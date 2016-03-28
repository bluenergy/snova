# 简介 #

gsnova也提供一个spac（SpecialProxyAutoConfig）用于更复杂的PAC场景。不过gsnova用JSON格式的规则描述文件，而非snova中用tykedog脚本。


# 运行机制 #

  * 默认的在gsnova安装目录下的spac子目录下，有两个规则文件user\_spac.json和cloud\_spac.json.
  * 默认开启，关闭需要修改gsnova.conf中`[SPAC]`中Enable为0
  * gsnova启动时解析两个规则文件；在运行过程中根据Proxy的HTTP请求优先匹配user\_spac.json中的规则，其次匹配cloud\_spac.json中的规则。根据匹配的结果选择代理实现。
  * 若所有的规则都不匹配，则选择gsnova.conf中`[SPAC]`下的Default配置的Proxy实现。

# 规则 #
  * spac规则文件由一组规则组成，规则之间的优先级按编写顺序排列。
  * 单个spac规则，由三部分组成：一为匹配条件，二为选择的Proxy实现，三为Proxy实现依赖的属性
> > ## 匹配条件 ##
      * 默认匹配条件也按匹配对象区分，目前支持Protocol， Method， URL， Host四类以及两个内置的条件IsBlockedByGFW，IsHostInCN。若有多个匹配条件，则匹配条件需要同时满足；单个匹配条件中若有多个规则，则只需要匹配一个即可满足。具体说明参考以下示例。
      * Host匹配规则用到最多，如下从默认cloud\_spac.json中摘出的一个简单规则:
```
        {
          "Host" : ["*.blogspot.*", "*.appspot.com", "golang.org"],
          "Proxy": ["GoogleHttps", "Default"]
        }
        含义为若代理请求的目的站点地址匹配其中任意一个"Host"，则选择“Proxy”中的实现。
       “Proxy”对应多个proxy时，参考下节说明
```
      * Protocol匹配规则仅用于http/https的匹配，如：
```
          {
           "Protocol" : "https",
           "Host": ["twitter.com"],
           "Proxy": ["Default"]
           }
          含义为若代理请求为https并且目的站点为“twitter.com”，则选择“Default”的Proxy实现。 
```
      * URL匹配规则用于代理请求的URL，如：
```
           {
             "URL" : ["www.google.*/imgres"],
             "Proxy":["Direct", "Default"]
           }
          含义为若代理请求的URL匹配“www.google.*/imgres”，则选择对应的Proxy实现。 
```
      * Method匹配条件用于匹配代理HTTP请求的类型，对应HTTP协议中的GET/POST/PUT/HEAD/DELETE等请求
      * 内置的IsBlockedByGFW。 IsBlockedByGFW为内置的GFWList判断, eg:
```
        {
        "Filter" : ["!IsBlockedByGFW"],
        "Proxy":["Direct", "Default"]
        }
        含义为若代理请求未被GFW阻断，则选择对应的Proxy实现。注意IsBlockedByGFW前有符号'!'代表IsBlockedByGFW判断失败的情况。
```
      * 内置的IsHostInCN。 IsHostInCN为内置的IP地址是否在CN的判断, eg:
```
        {
        "Filter" : ["IsHostInCN"],
        "Proxy":["Direct", "Default"]
        }
        含义为若代理请求的IP在CN，则选择对应的Proxy实现。注意IsHostInCN前也可增加符号'!'代表IsHostInCN判断失败的情况。
```
> > > > 同时注意，默认的需要将gsnova.conf中的IPRangeRepo配置项的注释放开才能生效IsHostInCN。


> ## Proxy实现 ##
    * 若Proxy选择有多个实现，则含义为在多个Proxy优先选择第一个Proxy，若失败，则尝试第二个；若仍失败，则尝试第三个，以此类推。失败目前仅包括连接不上，连接重置，超时等五应答的情况（不包括错误应答）。
    * 目前Proxy实现用特定名称代替，包括：
> > ### `GoogleHttps/GoogleHttp/Google` ###
      * 含义为直接连接Google的站点中转代理请求
> > ### Direct ###
      * 直接访问，不经过任何代理
> > ### GAE ###
      * 选择GAE的代理实现
> > ### C4 ###
      * 选择C4的代理实现
> > ### SSH ###
      * 选择SSH的代理实现
> > ### Default ###
      * gsnova.conf中`[SPAC]`下的Default配置的Proxy实现
> > ### 第三方Proxy ###
      * 第三方HTTP Proxy，如下：
```
            {
              "Host" : ["Target domain patterns for proxy"],
              "Proxy":["http://127.0.0.1:8080"]
            }
```
      * 第三方Socks5 Proxy，如下：
```
            {
              "Host" : ["Target domain patterns for proxy"],
              "Proxy":["socks://127.0.0.1:7070"]
            }
```


> ## Proxy属性 ##
    * Range
> > > 自动注入Range头，多并发下载
    * `RedirectHttps`
> > > 自动重定向至https

# 限制/不足 #
  * JSON解析限制严格，缺少一个符号会导致整个文件解析失败；修改规则文件需要特别注意
  * 只能用正则表达式匹配当前代理请求中的特定字符串以及简单的两个内置条件判断，无发基于上下文判断。（此项弱于snova的tykedog脚本）