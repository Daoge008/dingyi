\# 鼎一&平安财险API说明

\* Version : v-0.0.1

\* Author : xuewen.huang@qiwo.mobi

\* ChangeLog

   \* 2017/2/23 init



\#\#\# 概要

本文档由鼎一提供给安骑开发人员，帮助安骑快速接入平安的在线投保系统。  

\* 鼎一提供的API设计为RESTful风格，Request和Response数据全部使用json格式（电子保单查询返回为base64编码后的pdf文件二进制内容）。

\* 鼎一提供给安骑用于身份验证的apiKey和apiSecret，为了保证安全性，请安骑将其保存在服务器端，并在服务器端调用API。



\#\#\# 域名

\*  测试环境

  \* https://test-insurance.qiwocloud1.com

\* 生产环境

  \* https://insurance.qiwocloud1.com



\#\#\# 认证方式

\* 使用在HTTP Header 传入accessToken 的方式来验证调用方身份

   \* HTTP Header 示例

        \`\`\`

GET /v1/pingan/noPayRiskPropertyPolicies/10550003900150953229 HTTP/1.1

Host: test-insurance.qiwocloud1.com

Authorization: Bearer eyJhcGlLZXkiOiJhaTBDcmpsVGNCdmdwdnpkIiwidGltZXN0YW1wIjoxNDg3MzIwOTc3LCJ0b2tlbiI6IjUyNWM4YjI1NDFkNDQwY2QzYTJlMzdkMTI1YTYxNjYwZmE5YjQ4ZjYifQ

\`\`\`

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



\#\#\# API Response相关说明

\* HTTP status

 \* 200 ，表示请求成功

 \* 400 ，表示请求参数有误，请仔细核对文档的参数说明

 \* 401 ，表示身份验证失败，请检查是否按文档生成了accessToken

 \* 404 ，表示没有请求的资源

 \* 500 ，501，502等错误表示服务端错误，请重试或者联系服务开发人员。  

\* Response Body

    \* 所有API返回如下格式



       \`\`\`

{

  "errorCode": 0,

  "errorMessage": "",

  "data": {}

}

\`\`\`

   





\#\#\# API 列表



\#\#\#\# 1. 投保

   \`\`\`

POST /v1/pingan/noPayRiskPropertyPolicies

\`\`\`

\* 保单申请成功后，保单开始时间是第二天0点，结束时间是第二年的当天23点。

\* 示例

  \* Request



       \`\`\`

       curl -X POST -H "Authorization: Bearer eyJhcGlLZXkiOiJhaTBDc..."  -H "Content-Type: application/json" -d '{

	"applicantInfo":{   //投保人信息

		"certificateNo": "362426198704017713", //证件号码

                "certificateType": "01", //证件类型,01:身份证、02：护照、03：军人证、04：港澳通行证，05：驾驶证、06：港澳回乡证或台胞证，07：临时身份证、99：其他

               "birthday": "1992-05-18", //生日

               "sexCode": "F", //性别F,M

               "mobileTelephone": "15017924903", //手机号码

               "name": "李四" //姓名

	},

	"riskPropertyInfo":{ //标的信息

		"brandModel":"brandModel2", //厂牌型号

		"motorNo":"motorNo3",  //电动机号

		"vehicleLicenceCode":"vehicleLicenceCode3", //车牌号

		"vehicleFrameNo":"vehicleFrameNo4" //车架号

	},

	"optionProductPackage":"PK00003789", //可选盗抢险套餐编码 PK00003788 第一档，PK00003789 第二档，

	"insurantInfoList":\[ //被保险人列表

        {

            "age": "28", //年龄

            "birthday": "1988-04-01", //生日

            "certificateNo": "362426198704017712", //证件号码

            "certificateType": "01", //证件类型

            "mobileTelephone": "15017924903", //手机号

            "name": "王五", //姓名

            "sexCode": "M" //性别

        }

    \]

}' "https://test-insurance.qiwocloud1.com/v1/pingan/noPayRiskPropertyPolicies"

       \`\`\`



  \*  Response



      \`\`\`

 {

  "errorCode": 0,  //错误编码， 0表示没有错误

  "errorMessage": "", //错误信息

  "data": { //返回的结果数据

        "accidentApplyPolicyNoList": "", //驾意险特殊返回结果

        "isFee": "false", //是否见费出单

        "policyNo": "10550003900150953295", //保单号

        "status": "B5", //保单状态 BE:待审批，B1：待审核，BH：待验车，B2：待缴费，B3：待承保，B5：已承保

        "applyPolicyNo": "50550003900209623444",  //申请保单号

        "brokerFeeRate": "0.2", //经纪费比例

        "totalActualPremium": "60", //总费用

        "validateCode": "VGMn3gFRW29k6z44hu", //验证码

        "agentFeeRate": "0", //代理费比例

        "noticeNo": "", //通知编号

        "productCode": "MP02050088", //产品编码

        "applyResultDetail": \[\] //申请结果详情

     }

}

      \`\`\`



\#\#\#\# 2. 获取保单详情

   \`\`\`

GET /v1/pingan/noPayRiskPropertyPolicies/{$policyNo}

\`\`\`

\* 参数

   \* $policyNo，保单号，如 10550003900150953295。如传入其他字符，服务器将返回404。如格式正确，但系统没有此保单号，将返回200，errorCode=1111，data ={} 。

\* 示例

  \* Request



       \`\`\`

       curl -X GET -H "Authorization: Bearer eyJhcGlLZXkiOiJhaTBDc..."  "https://test-insurance.qiwocloud1.com/v1/pingan/noPayRiskPropertyPolicies/10550003900150953295"

       \`\`\`



  \*  Response

      

        成功：

        \`\`\`

 {

      "errorCode": 0,  //错误编码， 0表示没有错误

      "errorMessage": "", //错误信息

      "data": { //返回的保单详情

            ....

       }

  }

      \`\`\`

失败：

      \`\`\`

{

  "errorCode": 1111,

  "errorMessage": "查询数据为空",

  "data": {}

}

\`\`\`



\#\#\#\# 3. 获取电子保单PDF

   \`\`\`

GET /v1/pingan/noPayRiskPropertyPolicies/{$policyNo}.pdf

\`\`\`

\* 参数

   \* $policyNo，保单号，如 10550003900150953295。如传入其他字符，服务器将返回404。如格式正确，但系统没有此保单号，将返回200，errorCode=1111，data ={} 。

\* 示例

  \* Request



       \`\`\`

       curl -X GET -H "Authorization: Bearer eyJhcGlLZXkiOiJhaTBDc..."  "https://test-insurance.qiwocloud1.com/v1/pingan/noPayRiskPropertyPolicies/10550003900150953295.pdf"

       \`\`\`



  \*  Response  



         成功：

         \`\`\`

{

  "errorCode": 0,

  "errorMessage": "",

  "data": {

      "returnPdfValue" :"JVBERi0xLjQKJ....." //base64编码后的保单pdf内容

   }

}

\`\`\`

     失败：

      \`\`\`

{

  "errorCode": 1111,

  "errorMessage": "未查询到该合作伙伴的出单信息，请核对保单号！",

  "data": {}

}

  \`\`\`



\#\#\#\# 4. 注销保单 （已生效的保单不能注销）

   \`\`\`

DELETE /v1/pingan/noPayRiskPropertyPolicies/{$policyNo}

\`\`\`

\* 参数

   \* $policyNo，保单号，如 10550003900150953295。如传入其他字符，服务器将返回404。如格式正确，但系统没有此保单号，将返回200，errorCode=1112。 

\* 示例

  \* Request



       \`\`\`

       curl -X DELETE -H "Authorization: Bearer eyJhcGlLZXkiOiJhaTBDc..."  "https://test-insurance.qiwocloud1.com/v1/pingan/noPayRiskPropertyPolicies/10550003900150953295"

       \`\`\`



  \*  Response  



        成功：

        \`\`\`

{

  "errorCode": 0,

  "errorMessage": "",

  "data": {}

}

\`\`\`

     失败：

      \`\`\`

  {

  "errorCode": 1112,

  "errorMessage": "该单已退保/注销/契撤，不能再注销！",//获取他失败原因如：该单不存在。

  "data": {}

  }

  \`\`\`



