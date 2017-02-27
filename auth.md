# 认证方式

* 使用在HTTP Header 传入accessToken 的方式来验证调用方身份
* HTTP Header 示例

```
GET /v1/pingan/noPayRiskPropertyPolicies/10550003900150953229 HTTP/1.1
Host: test-insurance.qiwocloud1.com
Authorization: Bearer eyJhcGlLZXkiOiJhaTBDcmpsVGNCdmdwdnpkIiwidGltZXN0YW1wIjoxNDg3MzIwOTc3LCJ0b2tlbiI6IjUyNWM4YjI1NDFkNDQwY2QzYTJlMzdkMTI1YTYxNjYwZmE5YjQ4ZjYifQ
```

   Authorization: Bearer 后面的字符是accessToken

*  accessToken 生成方式

     1. 生成如下json串： {"apiKey":$apiKey,"timestamp":$timestamp,"token":sha1\($apiSecret.$timestamp\)} 。

     2. 将生成好的json串base64编码，就是accessToken。

*  accessToken生成示例\(PHP\)



```
$apiKey = "CPDc9mZClWkTjIPS";//鼎一提供给安骑的api键值
$apiSecret = "xiJXocuFZz8zqpc0U3k5TSJCDUNTpTV8"; //鼎一提供给安骑的api密钥
$timestamp = time(); //当前的时间戳, 生产环境会验证传入的时间戳是否和鼎一服务器时间戳相差在100秒以内
$t = array("apiKey"=>$apiKey,"timestamp"=>$timeStamp,"token"=>sha1($apiSecret.$timestamp));
$accessToken =  base64_encode(json_encode($t));
```



