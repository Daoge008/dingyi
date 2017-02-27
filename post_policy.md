```
# 投保
```

    Methods allow you to smoothly display code examples in different languages.

    {% method %}
    ## POST /v1/pingan/noPayRiskPropertyPolicies

    保单申请成功后，保单开始时间是第二天0点，结束时间是第二年的当天23点。

    * 保单申请成功后，保单开始时间是第二天0点，结束时间是第二年的当天23点。
    * 示例
      * Request

           ```bash
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
        "insurantInfoList":[ //被保险人列表
            {
                "age": "28", //年龄
                "birthday": "1988-04-01", //生日
                "certificateNo": "362426198704017712", //证件号码
                "certificateType": "01", //证件类型
                "mobileTelephone": "15017924903", //手机号
                "name": "王五", //姓名
                "sexCode": "M" //性别
            }
        ]
    }' "https://test-insurance.qiwocloud1.com/v1/pingan/noPayRiskPropertyPolicies"
           ```

      *  Response

          ```
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
            "applyResultDetail": [] //申请结果详情
         }
    }
          ```



