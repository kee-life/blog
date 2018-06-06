---
title: jstorm-crawler
date: 2017-04-25 19:42:38
categories:
  - jstorm
tags:
  - storm
---
### Config

jstorm-crawler configuration parameters
:smile: :laughing:

配置项 | 默认值 |  使用类 | 描述
:--------------------------------|:------------------------------|:---------------------------|:----------------------------------------
fetcher.threads.per.queue        | 1                             |FeatcherBolt.java           | 每个爬取url队列并发线程数                
fetcher.queue.mode               | "byHost" ("byDomain", "byIP") |FeatcherBolt.java           | 按照什么规则来区分队列                   
fetcher.server.delay             | 1.0f                          |FeatcherBolt.java           | 取url延时                                
fetcher.server.min.delay         | 0.0f                          |FeatcherBolt.java           | 去url最小延时（当线程数>1,使用此延时)    
fetcher.maxThreads.${id}         | 1                             |FeatcherBolt.java           | 定制化的某个id最大并发线程数             
fetcher.max.crawl.delay          | 30                            |FeatcherBolt.java           | 最大延时                                 
fetcher.metrics.time.bucket.secs | 10                            |FeatcherBolt.java           | 间隔几秒，输出一次Metric信息             
fetcherbolt.queue.debug.filepath |                               |FeatcherBolt.java           | queue内容输出文件路径                    
fetcher.threads.number           | 10                            |FeatcherBolt.java           | 爬取并发线程数                           
sitemap.discovery                | false                         |FeatcherBolt.java           | 是否取sitemap中信息                      
fetcher.max.urls.in.queues       | -1                            |FeatcherBolt.java           | url队列中最大url数                       
sitemap.sniffContent             | false                         |SiteMapParserBolt.java      | 如果metaData里无isSiteMap=true,还是否解析
sitemap.filter.hours.since.modified | -1                         |SiteMapParserBolt.java      | 过滤掉Last-Modified是几个小时之前的sitemap
sitemap.offset.guess             | 300                           |SiteMapParserBolt.java   | 从网页内容猜测该url是否是sitemap的最大字数
parser.emitOutlinks              | true                          |                            |                                          
track.anchors                    | true                          |                            |                                          
robots.noFollow.strict           | true                          |                            | 是否严格遵守robots.txt 中noFollow指示    
jsoup.treat.non.html.as.error    | true                          |                            |                                          
detect.mimetype                  | true                          |                            |                                          
detect.charset.maxlength         | -1                            |                            |                                          
parsefilters.config.file         |                               |ParseFilters.java           | 页面抽取配置文件                         
http.skip.robots                 | false                         |AbstractHttpProtocol.java   | 是否忽略robots.txt                       
http.store.headers               | false                         |AbstractHttpProtocol.java   | 是否存储头部                             
http.use.cookies                 | false                         |AbstractHttpProtocol.java   | 是否使用cookies                          
fetcher.threads.number           | 200                           |HttpProtocol.java           | 并发线程                                 
http.content.limit               | -1                            |HttpProtocol.java           | 下载大小限制                             
http.timeout                     | 10000                         |HttpProtocol.java           | 下载超时时间                             
http.proxy.host                  | null                          |HttpProtocol.java           | 代理地址                                 
http.proxy.port                  | 8080                          |HttpProtocol.java           | 代理端口                                 
http.proxy.user                  | null                          |HttpProtocol.java           | 代理用户名                               
http.proxy.pass                  | null                          |HttpProtocol.java           | 代理用户密码                             
parser.emitOutlinks              | true                          |JSoupParserBolt.java        | 是否处理外链                             
track.anchors                    | true                          |JSoupParserBolt.java        | 是否跟踪锚点                             
robots.noFollow.strict           | true                          |JSoupParserBolt.java        | 是否严格执行robots.txt noFollow规则      
jsoup.treat.non.html.as.error    | true                          |JSoupParserBolt.java        | 非html文档是否当成错误                   
detect.mimetype                  | true                          |JSoupParserBolt.java        | 是否检测mimetype                         
detect.charset.maxLength         | -1                            |JSoupParserBolt.java        | 最长检测字符数                           



