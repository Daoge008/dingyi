# API Response相关说明

*  HTTP status

        200 ，表示请求成功

        400 ，表示请求参数有误，请仔细核对文档的参数说明

        401 ，表示身份验证失败，请检查是否按文档生成了accessToken

        404 ，表示没有请求的资源

        500 ，501，502等错误表示服务端错误，请重试或者联系服务开发人员。  

* Response Body

           所有API返回如下格式

```js
{

"errorCode": 0,

"errorMessage": "",

"data": {}

}
```



