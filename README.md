#  鼎一&平安财险API说明

*  Version : v-0.0.1

*   Author : xuewen.huang@qiwo.mobi

*  ChangeLog

*   2017/2/23 init

### 概要 

本文档由鼎一提供给安骑开发人员，帮助安骑快速接入平安的在线投保系统。  

鼎一提供的API设计为RESTful风格，Request和Response数据全部使用json格式（电子保单查询返回为base64编码后的pdf文件二进制内容）。

鼎一提供给安骑用于身份验证的apiKey和apiSecret，为了保证安全性，请安骑将其保存在服务器端，并在服务器端调用API。

### 域名 

* 测试环境

         https://test-insurance.qiwocloud1.com

*  生产环境

         https://insurance.qiwocloud1.com



### 认证方式

*  使用在HTTP Header 传入accessToken 的方式来验证调用方身份

*   HTTP Header 示例

```
GET /v1/pingan/noPayRiskPropertyPolicies/10550003900150953229 HTTP/1.1
Host: test-insurance.qiwocloud1.com
Authorization: Bearer eyJhcGlLZXkiOiJhaTBDcmpsVGNCdmdwdnpkIiwidGltZXN0YW1wIjoxNDg3MzIwOTc3LCJ0b2tlbiI6IjUyNWM4YjI1NDFkNDQwY2QzYTJlMzdkMTI1YTYxNjYwZmE5YjQ4ZjYifQ
```



Authorization: Bearer 后面的字符是accessToken

\* accessToken 生成方式

     1. 生成如下json串： {"apiKey":$apiKey,"timestamp":$timestamp,"token":sha1\($apiSecret.$timestamp\)} 。

     2. 将生成好的json串base64编码，就是accessToken。

\* accessToken生成示例\(PHP\)



        \`\`\`  php

        $apiKey = "CPDc9mZClWkTjIPS";//鼎一提供给安骑的api键值

        $apiSecret = "xiJXocuFZz8zqpc0U3k5TSJCDUNTpTV8"; //鼎一提供给安骑的api密钥

        $timestamp = time\(\); //当前的时间戳, 生产环境会验证传入的时间戳是否和鼎一服务器时间戳相差在100秒以内

        $t = array\("apiKey"=&gt;$apiKey,"timestamp"=&gt;$timeStamp,"token"=&gt;sha1\($apiSecret.$timestamp\)\);

        $accessToken =  base64\_encode\(json\_encode\($t\)\);

\`\`\`

