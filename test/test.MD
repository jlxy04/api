# 接口说明

>本地环境请求前缀：http://127.0.0.1:8081

>测试环境请求前缀：http://rehab-api-te.dongzhengyi.com

>所有接口都必须带上默认的参数：token

## 错误码

- [x]  登录相关

100001 - token不合法

100002 - 注册待审核

100003 - 资料待完善

- [x]  账户相关

200001 - 已存在提现申请

200002 - 提现金额必须大于0

- [x]  订单相关

300001 - 订单已被领取

300002 - 订单不存在

300003 - 非自己订单不能撤回

300004 - 订单非可撤回状态

- [x]  医生相关

400001 - 医生{}不存在

- [x]  商品相关

500001 - 商品{}不存在

- [x]  验证码相关

900003 - 验证码已失效

900004 - 验证码错误

900005 - 手机号码已注册

900006 - doctorCode不存在

## 枚举字

### 医生状态

TOBEFILL - 待补充资料

PAUDIT - 待审核
	
AUDITED - 已审核

### 订单状态

CLAIM - 待领取
	
PUBLICITY - 公示中

CLAIMED - 订单完成
	
## 医生服务

### 根据手机号码查询医生信息
> 示例url：http://127.0.0.1:8081/api/doctor/getDoctorByMobile

> Method：POST

> 请求参数：mobile 必输

> 返回数据示例：
```
{"auditStatus":"PAUDIT","auditTime":1505350123000,"code":"10002","createTime":1505352693000,"depCode":"1","depName":"1","hospitalCode":"1","hospitalName":"1","mobile":"13725579628","modifyTime":1506691424000,"name":"1","openid":"123456","professionCode":"1","professionName":"1","province":"1","recoveredCode":"1"}

```


### 根据openid查询医生信息
> 示例url：http://127.0.0.1:8081/api/doctor/getDoctorByOpenid

> Method：POST

> 请求参数：openid 必输

> 返回数据示例：
```
{"auditTime":1505352750000,"code":"10000","createTime":1506518621000,"depCode":"1","depName":"1","hospitalCode":"1","hospitalName":"1","mobile":"1","modifyTime":1506691424000,"name":"1","openid":"1234568","professionCode":"1","professionName":"1","province":"1","recoveredCode":"1"}
```

### 根据医生编码查询医生信息
> 示例url：http://127.0.0.1:9090/api/doctor/getDoctorByCode

> Method：POST

> 请求参数：doctorCode（医生编码） 必输

> 返回数据示例：
```
{
    "auditStatus": "TOBEFILL",
    "code": "10018",
    "createTime": 1510315561000,
    "depCode": "",
    "depName": "",
    "hospitalCode": "",
    "hospitalName": "",
    "mobile": "13725579626",
    "modifyTime": 1510316500000,
    "name": "张三",
    "openid": "oXZ0xwaQ747CboMJkxqACQ4vp000",
    "professionCode": "",
    "professionName": "",
    "province": "湖南省",
    "recoveredCode": "",
    "recoveredName": "",
    "recoveredSubCode": "",
    "recoveredSubName": "",
    "statistics": {
        "attentionCount": 1,
        "evaluateCount": 12,
        "fansCount": 1,
        "likeCount": 1212,
        "readCount": 1211
    }
}
```


### 注册并完善医生信息接口

> 示例url：http://127.0.0.1:8081/api/doctor/regist

> Method：POST

> 请求参数：

- jsonStr (注册json字符串) 必输，示例如下：

```
{"depCode":"","depName":"","hospitalCode":"","hospitalName":"","mobile":"13725579629","name":"张三","openid":"oXZ0xwaQ747CboMJkxqACQ4vp000","professionCode":"","professionName":"","province":"湖南省","recoveredCode":"","recoveredName":"","recoveredSubCode":"","recoveredSubName":""}
```


> 返回数据示例：

```
{"auditStatus":"PAUDIT","code":"10004","createTime":1505373778402,"depCode":"","depName":"","hospitalCode":"","hospitalName":"","mobile":"13725579639","name":"张三","openid":"oXZ0xwaQ747CboMJkxqACQ4vp000X","professionCode":"","professionName":"","province":"湖南省","recoveredCode":"","recoveredName":"","recoveredSubCode":"","recoveredSubName":""}

```

### 通过手机登录接口
> 通过手机号码登录，如果手机已存在并审核通过，则登录返回token; 如果手机未注册，则注册会员，返回会员信息

> 示例url：http://127.0.0.1:8081/api/doctor/registOrLogin

> Method：POST

> 请求参数：

- mobile 手机号码(必输)
- smsCode 验证码(必输)

> 返回数据示例：

```
{"auditStatus":"TOBEFILL","code":"10018","createTime":1510315561266,"mobile":"13725579626"}

```

### 完善资料接口
> 注册成功后，完善资料接口

> 示例url：http://127.0.0.1:9090/api/doctor/fillData

> Method：POST

> 请求参数：

- jsonStr 资料json字符串

```
jsonStr	{"depCode":"","depName":"","hospitalCode":"","hospitalName":"","mobile":"13725579626","name":"张三","openid":"oXZ0xwaQ747CboMJkxqACQ4vp000","professionCode":"","professionName":"","province":"湖南省","recoveredCode":"","recoveredName":"","recoveredSubCode":"","recoveredSubName":""}
```

> 返回数据示例：

```
{"auditStatus":"PAUDIT","code":"10018","depCode":"","depName":"","hospitalCode":"","hospitalName":"","modifyTime":1510316500095,"name":"张三","openid":"oXZ0xwaQ747CboMJkxqACQ4vp000","professionCode":"","professionName":"","province":"湖南省","recoveredCode":"","recoveredName":"","recoveredSubCode":"","recoveredSubName":""}
```
### 通过token登录接口
> 通过手机号码登录成功后，缓存token至本地，后面每次进来时用token进行一次登录

> 示例url：http://127.0.0.1:8081/api/doctor/tokenLogin

> Method：POST

> 请求参数：

- token token(必输)

> 返回数据示例：

```
{"code":"10014","mobile":"13725579627","name":"李四"}

```


### 查询医生状态

> 示例url：http://127.0.0.1:9090/api/doctor/getDoctorAuditStatus

> Method：POST

> 请求参数：

- doctorCode 医生编码

```
"PAUDIT"
```

> 返回数据示例：

```
{"auditStatus":"TOBEFILL","code":"10018","createTime":1510315561266,"mobile":"13725579626"}
```

### 登录
> 登录返回token

> 示例url：http://127.0.0.1:8081/api/doctor/login

> Method：POST

> 请求参数：

- mobile 手机号码（必输） 

- smsCode 验证码（必输）


> 返回数据示例：

```
{"code":"10011","mobile":"13725579627","name":"李五4","token":"O/2e91orKqkGidVrYPU53q81KK7nGhw3CEKC3f3Mc2Dm+vcpq9BE4dClrUPZ/VuHlNrBKtOKrnhdYD4l8Gr1hQ=="}

```

## 字典相关
### 查询最顶层职业
> 示例URL：http://127.0.0.1:8081/api/dict/listTopProfession

> 请求参数：无

> Method：POST

> 返回数据示例

```
[{"classifyCode":"profession","code":"10000","name":"医生"},{"classifyCode":"profession","code":"10001","name":"治疗师","parentCode":"","remark":""},{"classifyCode":"profession","code":"10002","name":"护士","parentCode":"","remark":""}]
```

### 查询所有职业
> 示例URL：http://127.0.0.1:8081/api/dict/listAllProfession

> 请求参数：无

> Method：POST

> 返回数据示例

```
[{"classifyCode":"profession","code":"10000","name":"医生"},{"classifyCode":"profession","code":"10001","name":"治疗师","parentCode":"","remark":""},{"classifyCode":"profession","code":"10002","name":"护士","parentCode":"","remark":""},{"classifyCode":"profession","code":"10003","name":"主任医生","parentCode":"10000","remark":""},{"classifyCode":"profession","code":"10004","name":"副主任医生","parentCode":"10000","remark":""},{"classifyCode":"profession","code":"10005","name":"主治医生","parentCode":"10000","remark":""},{"classifyCode":"profession","code":"10006","name":"住院医生","parentCode":"10000","remark":""},{"classifyCode":"profession","code":"10007","name":"主任护士","parentCode":"10002","remark":""},{"classifyCode":"profession","code":"10008","name":"副主任护士","parentCode":"10002","remark":""},{"classifyCode":"profession","code":"10009","name":"主管护士","parentCode":"10002","remark":""},{"classifyCode":"profession","code":"10010","name":"护师","parentCode":"10002","remark":""},{"classifyCode":"profession","code":"10011","name":"护士","parentCode":"10002","remark":""},{"classifyCode":"profession","code":"10012","name":"成人康复","parentCode":"10001","remark":""},{"classifyCode":"profession","code":"10013","name":"儿童康复","parentCode":"10001","remark":""},{"classifyCode":"profession","code":"10014","name":"物理疗法（PT）","parentCode":"10012","remark":"物理治疗、体育疗法、运动疗法"},{"classifyCode":"profession","code":"10024","name":"物理疗法（PT）","parentCode":"10013","remark":"物理治疗、体育疗法、运动疗法"}]
```

### 查询最顶层科室
> 示例URL：http://127.0.0.1:8081/api/dict/listTopDep

> 请求参数：无

> Method：POST

> 返回数据示例

```
[{"classifyCode":"dep","code":"20000","name":"康复科"},{"classifyCode":"dep","code":"20001","name":"骨科"},{"classifyCode":"dep","code":"20002","name":"神经外科","parentCode":"","remark":""},{"classifyCode":"dep","code":"20003","name":"神经内科","parentCode":"","remark":""},{"classifyCode":"dep","code":"20004","name":"妇科","parentCode":"","remark":""}]
```

### 查询所有科室
> 示例URL：http://127.0.0.1:8081/api/dict/listAllDep

> 请求参数：无

> Method：POST

> 返回数据示例

```
[{"classifyCode":"dep","code":"20000","name":"康复科"},{"classifyCode":"dep","code":"20001","name":"骨科"},{"classifyCode":"dep","code":"20002","name":"神经外科","parentCode":"","remark":""},{"classifyCode":"dep","code":"20003","name":"神经内科","parentCode":"","remark":""},{"classifyCode":"dep","code":"20004","name":"妇科","parentCode":"","remark":""}]
```


## 查询下级字典
主要用于查询下级职业或下级科室
> 示例URL：http://127.0.0.1:8081/api/dict/listChildDict

> 请求参数：parentCode 必输

> Method：POST

> 返回数据示例

```
[{"classifyCode":"profession","code":"10003","name":"主任医生","parentCode":"10000","remark":""},{"classifyCode":"profession","code":"10004","name":"副主任医生","parentCode":"10000","remark":""},{"classifyCode":"profession","code":"10005","name":"主治医生","parentCode":"10000","remark":""},{"classifyCode":"profession","code":"10006","name":"住院医生","parentCode":"10000","remark":""}]
```
## 查询适用人员
主要用于增加点评时显示适用人员
> 示例URL：http://localhost:9090/api/dict/listAllApplyPerson

> 请求参数：无

> Method：POST

> 返回数据示例

```
[
    {
        "code": "30001",
        "name": "物理"
    },
    {
        "code": "30002",
        "name": "作业"
    },
    {
        "code": "30003",
        "name": "言语"
    },
    {
        "code": "30004",
        "name": "心理"
    },
    {
        "code": "30005",
        "name": "工程"
    },
    {
        "code": "30006",
        "name": "职业疗"
    },
    {
        "code": "30007",
        "name": "中医"
    },
    {
        "code": "30008",
        "name": "多种"
    }
]
```

## 医院信息查询
### 根据输入标识模糊查询医院信息

> 示例URL：http://127.0.0.1:8081/api/hospital/listByCode

> 请求参数：  
- code (搜索字符) 必输

> Method：POST

> 返回数据示例

```
[{"address":"长沙市湘雅路141号","code":"21167","grade":"三甲","name":"中南大学湘雅医院","province":"湖南省","specialDep":"神经内科、普外科、烧伤科、神经外科、耳鼻喉科、血液科、中西医结合科、传染科等"},{"address":"长沙市人民中路86号","code":"21168","grade":"三甲","name":"中南大学湘雅二医院","province":"湖南省","specialDep":"内分泌科、心血管内科、眼科、肝病科、心胸外科、心内科、普外科、脊柱外科、妇产科、肾脏病内科、儿科、泌尿外科等"},{"address":"长沙市河西桐梓坡路","code":"21169","grade":"三甲","name":"中南大学湘雅三医院","province":"湖南省","specialDep":"儿科、骨科、泌尿外科、耳鼻喉科、消化内科、呼吸内科、妇产科、心血管内科、烧伤整形科、神经外科、神经内科、普外科、肾脏外科等"}]
```

## 商品服务

### 根据商品编码查询

> 示例URL：http://127.0.0.1:8081/api/goods/getGoods

> 请求参数：  
- goodsCode 必输

> Method：POST

> 返回数据示例

```
{
"code":"DZ00331",
"goodsImgList":[
{
"combine":"https://img.yzcdn.cn/upload_files/2017/11/21/Fg_dBrgZ60wHXTmcIWtaGr_uZ7so.jpg?imageView2/2/w/600/h/0/q/75/format/jpg",
"goodsCode":"DZ00331",
"id":45481,
"thumbnail":"https://img.yzcdn.cn/upload_files/2017/11/21/Fg_dBrgZ60wHXTmcIWtaGr_uZ7so.jpg?imageView2/2/w/290/h/290/q/75/format/jpg",
"url":"https://img.yzcdn.cn/upload_files/2017/11/21/Fg_dBrgZ60wHXTmcIWtaGr_uZ7so.jpg"
},
{
"combine":"https://img.yzcdn.cn/upload_files/2017/11/21/FkmUnuZr-EBEfeUHvrk_xWON3WDa.jpg?imageView2/2/w/600/h/0/q/75/format/jpg",
"goodsCode":"DZ00331",
"id":45482,
"thumbnail":"https://img.yzcdn.cn/upload_files/2017/11/21/FkmUnuZr-EBEfeUHvrk_xWON3WDa.jpg?imageView2/2/w/290/h/290/q/75/format/jpg",
"url":"https://img.yzcdn.cn/upload_files/2017/11/21/FkmUnuZr-EBEfeUHvrk_xWON3WDa.jpg"
},
{
"combine":"https://img.yzcdn.cn/upload_files/2017/11/21/FpvgKCiv1jcgyM7b-MnjK12ufbGr.jpg?imageView2/2/w/600/h/0/q/75/format/jpg",
"goodsCode":"DZ00331",
"id":45483,
"thumbnail":"https://img.yzcdn.cn/upload_files/2017/11/21/FpvgKCiv1jcgyM7b-MnjK12ufbGr.jpg?imageView2/2/w/290/h/290/q/75/format/jpg",
"url":"https://img.yzcdn.cn/upload_files/2017/11/21/FpvgKCiv1jcgyM7b-MnjK12ufbGr.jpg"
},
{
"combine":"https://img.yzcdn.cn/upload_files/2017/11/21/FsULa-6-m2iPU_zyyJxvmyfGewHJ.jpg?imageView2/2/w/600/h/0/q/75/format/jpg",
"goodsCode":"DZ00331",
"id":45484,
"thumbnail":"https://img.yzcdn.cn/upload_files/2017/11/21/FsULa-6-m2iPU_zyyJxvmyfGewHJ.jpg?imageView2/2/w/290/h/290/q/75/format/jpg",
"url":"https://img.yzcdn.cn/upload_files/2017/11/21/FsULa-6-m2iPU_zyyJxvmyfGewHJ.jpg"
},
{
"combine":"https://img.yzcdn.cn/upload_files/2017/11/21/Fu8QFgNypP57T0j4xd8B1vH8u_G9.jpg?imageView2/2/w/600/h/0/q/75/format/jpg",
"goodsCode":"DZ00331",
"id":45485,
"thumbnail":"https://img.yzcdn.cn/upload_files/2017/11/21/Fu8QFgNypP57T0j4xd8B1vH8u_G9.jpg?imageView2/2/w/290/h/290/q/75/format/jpg",
"url":"https://img.yzcdn.cn/upload_files/2017/11/21/Fu8QFgNypP57T0j4xd8B1vH8u_G9.jpg"
}
],
"name":"ober 踝关节固定支具小腿脚踝骨折护具矫正内翻外翻足下垂护托",
"originPrice":"市场价：￥178",
"picThumbUrl":"https://img.yzcdn.cn/upload_files/2017/11/21/Fg_dBrgZ60wHXTmcIWtaGr_uZ7so.jpg!120x120.jpg",
"retailPrice":140,
"salesClassCode":"A",
"sellPoint":"ober 踝关节固定支具小腿脚踝骨折护具矫正内翻外翻足下垂护托",
"sellUrl":"https://h5.youzan.com/v2/showcase/goods?alias=2xdz2k0vcccjh&from=wsc&kdtfrom=wsc"
}
```
> 部分返回字段解释 
- retailPrice - 售价
- originPrice - 原价
- sellPoint - 卖点
- salesClassCode - 销售分类编码
- sellUrl - 销售地址
- picThumbUrl - 主图缩略图
 
- url - 图片链接地址
- thumbnail - 图片缩略图链接地址


### 查询类目下所有商品

> 示例URL：http://127.0.0.1:8081/api/goods/listByCategCode

> 请求参数：  
- categoriesId 必输（类目ID）
- source   必输（类目来源）

> Method：POST

> 返回数据示例
```
[
    {
        "code": "DZ00853",
        "name": "永辉颈椎牵引器家用颈部牵引椅医用颈椎治疗仪吊脖矫正拉伸牵引架",
        "originPrice": "市场价：￥228",
        "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/24/Fho21OM-h5UPlYeSGNP_SeulAgM6.jpg!120x120.jpg",
        "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
        "retailPrice": 218.97,
        "salesClassCode": "B",
        "sellPoint": "永辉颈椎牵引器家用颈部牵引椅医用颈椎治疗仪吊脖矫正拉伸牵引架",
        "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=2ossu9jl72hzh&from=wsc&kdtfrom=wsc",
        "statistics": {
            "evaluateCount": 0,
            "likeCount": 0,
            "readCount": 0
        }
    },
    {
        "code": "DZ00860",
        "name": "永辉器械牵引床人体拉伸器颈椎腰椎牵引床腰椎间盘突出牵引机",
        "originPrice": "市场价：￥920",
        "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/24/FlUEd3kVE9-NweAEGuECA0KQFR6k.jpg!120x120.jpg",
        "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
        "retailPrice": 901.6,
        "salesClassCode": "B",
        "sellPoint": "永辉器械牵引床人体拉伸器颈椎腰椎牵引床腰椎间盘突出牵引机",
        "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=3ez4vuez68cal&from=wsc&kdtfrom=wsc",
        "statistics": {
            "evaluateCount": 0,
            "likeCount": 0,
            "readCount": 0
        }
    },
    {
        "code": "DZ00869",
        "name": "永辉人体拉伸器颈椎腰椎牵引床腰椎间盘牵引床器家用腰椎间盘突出",
        "originPrice": "市场价：￥428",
        "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/24/FpP3pkdkg8xijvgzbf2kjuiutgeU.jpg!120x120.jpg",
        "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
        "retailPrice": 419.44,
        "salesClassCode": "B",
        "sellPoint": "永辉人体拉伸器颈椎腰椎牵引床腰椎间盘牵引床器家用腰椎间盘突出",
        "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=1yco4m6wok8y5&from=wsc&kdtfrom=wsc",
        "statistics": {
            "evaluateCount": 0,
            "likeCount": 0,
            "readCount": 0
        }
    },
    {
        "code": "DZ01002",
        "name": "佳禾 全绒充气式颈托 家用颈椎牵引器",
        "originPrice": "市场价：￥166",
        "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/24/Fvbqpm0Ky9UZlClzys1MfxL-r8rI.jpg!120x120.jpg",
        "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
        "retailPrice": 65,
        "salesClassCode": "A",
        "sellPoint": "佳禾 全绒充气式颈托 家用颈椎牵引器",
        "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=2op45r51au825&from=wsc&kdtfrom=wsc",
        "statistics": {
            "evaluateCount": 0,
            "likeCount": 0,
            "readCount": 0
        }
    },
    {
        "code": "DZ01004",
        "name": "佳禾JBQ-03充气颈托 家用颈椎牵引器",
        "originPrice": "市场价：￥256",
        "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/24/Fhip55UQwrHZpOMOfxFzYwTkVIPr.jpg!120x120.jpg",
        "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
        "retailPrice": 96,
        "salesClassCode": "A",
        "sellPoint": "佳禾JBQ-03充气颈托 家用颈椎牵引器",
        "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=2x92e735bfk71&from=wsc&kdtfrom=wsc",
        "statistics": {
            "evaluateCount": 0,
            "likeCount": 0,
            "readCount": 0
        }
    },
    {
        "code": "DZ01201",
        "name": "舒尔美充气颈椎牵引器家用颈部拉伸按摩器 护颈托矫正治疗器成人",
        "originPrice": "市场价：￥169",
        "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/24/FtYMkUjh5JtDPHjUrW18RIPXHNLr.jpg!120x120.jpg",
        "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
        "retailPrice": 165.62,
        "salesClassCode": "B",
        "sellPoint": "舒尔美充气颈椎牵引器家用颈部拉伸按摩器 护颈托矫正治疗器成人",
        "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=1ygdbze6qqovh&from=wsc&kdtfrom=wsc",
        "statistics": {
            "evaluateCount": 0,
            "likeCount": 0,
            "readCount": 0
        }
    },
    {
        "code": "DZ18007",
        "name": "腰椎牵引训练器按摩拉伸器康复器材腰椎间盘劳损突出家用腰部牵引",
        "originPrice": "市场价：￥60.9",
        "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/12/26/FnN901fBhGg-pP7T4wgBVERwX5ez.jpg!120x120.jpg",
        "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
        "retailPrice": 58,
        "sellPoint": "腰椎牵引训练器按摩拉伸器康复器材腰椎间盘劳损突出家用腰部牵引",
        "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=3eqjorqh3qr4d&from=wsc&kdtfrom=wsc",
        "statistics": {
            "evaluateCount": 0,
            "likeCount": 0,
            "readCount": 0
        }
    }
]
```
>部分返回字段说明
- statistics 统计信息
 - evaluateCount 点评数量
 - likeCount 喜欢数量 
 - readCount 阅读数量

### 查询商品型号/颜色

> 示例URL：http://127.0.0.1:8081/api/goods/listSkuByGoodsCode

> 请求参数：  
- goodsCode 商品编码 (必输)

> Method：POST

> 返回数据示例

```
[
    {
        "name": "左-S",
        "skuId": "36183672"
    },
    {
        "name": "左-M",
        "skuId": "36183673"
    },
    {
        "name": "左-L",
        "skuId": "36183674"
    },
    {
        "name": "右-S",
        "skuId": "36183675"
    },
    {
        "name": "右-M",
        "skuId": "36183676"
    },
    {
        "name": "右-L",
        "skuId": "36183677"
    }
]
```
## 类目服务

### 查询所有类目

> 示例URL：http://127.0.0.1:8081/api/categories/listAll

> 请求参数：  
- 无参数

> Method：POST

> 返回数据示例

```
[
{
"id":1,
"name":"站立斜板",
"source":"YOUZAN"
},
{
"id":2,
"name":"站立床",
"source":"YOUZAN"
},
{
"id":3,
"name":"轮椅",
"source":"YOUZAN"
}
]
```
### 分页查询类目

> 示例URL：http://127.0.0.1:8081/api/categories/listByPage

> 请求参数：  
- pageNum 必输 （页编码）
- pageSize 必输 （页大小）

> Method：POST

> 返回数据示例

```
{
    "count": 40,
    "dataList": [
        {
            "goods": [
                {
                    "code": "DZ00955",
                    "name": "玖健康复机配件底座足内翻足外翻站立斜板",
                    "originPrice": "市场价：￥300",
                    "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/24/FlYyRwpbKNiRBR2oY8S7j0dBrvOZ.jpg!120x120.jpg",
                    "retailPrice": 294,
                    "salesClassCode": "B",
                    "sellPoint": "玖健康复机配件底座足内翻足外翻站立斜板",
                    "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=2fz23ti6cvrr1&from=wsc&kdtfrom=wsc"
                }
            ],
            "id": 1,
            "name": "站立斜板",
            "source": "YOUZAN"
        },
        {
            "goods": [
                {
                    "code": "DZ00073",
                    "name": "东正益康复 家用电动站立床 脚踏板屈伸角度可调",
                    "originPrice": "市场价：￥1896",
                    "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/21/FnTV7l4uy4Zh69hU7EYqKTEwOW_t.jpg!120x120.jpg",
                    "retailPrice": 1780,
                    "salesClassCode": "A",
                    "sellPoint": "东正益康复 家用电动站立床 脚踏板屈伸角度可调",
                    "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=3nu5ivp7mfmot&from=wsc&kdtfrom=wsc"
                }
            ],
            "id": 2,
            "name": "站立床",
            "source": "YOUZAN"
        }
    ]
}
```
> 部分返回字段解释 
- count - 类目总数
- retailPrice - 售价
- originPrice - 原价
- sellPoint - 卖点
- salesClassCode - 销售分类编码
- sellUrl - 销售地址
- picThumbUrl - 主图缩略图

## 订单服务

### 根据医生编码查询订单

> 示例URL：http://127.0.0.1:8081/api/order/listByDoctorCode

> 请求参数：  
- doctorCode 必输
- orderStatus 非必输, 枚举值：CLAIM-待领取、CLAIMED-已领取

> Method：POST

> 返回数据示例

```
[{"actuallyPrice":1.00,"code":"100001","createTime":1505485045000,"freightPrice":1.00,"receiverAddress":"1","receiverMobile":"1","receiverName":"1","remark":"1","source":"1","status":"CLAIM"}]
```

### 查询所有未领取的订单

> 示例URL：http://127.0.0.1:8081/api/order/listClaimOrder

> 请求参数：  无

> Method：POST

> 返回数据示例

```
[
{
"actuallyPrice":49,
"appeal":false,
"code":"100090",
"createTime":1513394331000,
"freightPrice":0,
"goodsUrl":"https://h5.youzan.com/v2/showcase/goods?alias=3nglqagafc6el&from=wsc&kdtfrom=wsc",
"orderCreateTime":1512475998000,
"orderTitle":"佳禾护膝保暖男女士老寒腿自发热四季老年人膝盖关节防寒冬季加厚",
"picThumbUrl":"https://img.yzcdn.cn/upload_files/2017/12/11/Fs1W9zvwnFOJxwuExkIxrH6ZB1-o.jpg!120x120.jpg",
"receiverAddress":"湖南省长沙市芙蓉区******",
"receiverMobile":"'13825253477",
"receiverName":"傅**",
"remark":"'null",
"source":"TAOBAO",
"status":"CLAIM"
},
{
"actuallyPrice":147,
"appeal":false,
"code":"100091",
"createTime":1513394331000,
"freightPrice":0,
"goodsUrl":"https://h5.youzan.com/v2/showcase/goods?alias=1y7rm8zd4j0nx&from=wsc&kdtfrom=wsc",
"orderCreateTime":1512468798000,
"orderTitle":"Ober CO-10肩托 固定肩关节",
"picThumbUrl":"https://img.yzcdn.cn/upload_files/2017/11/24/FtVqNu91iEJfPEeVWw_jh5elnm2l.jpg!120x120.jpg",
"receiverAddress":"安徽省亳州市谯城区亳州市人民医院南院6号楼一楼西头康****",
"receiverMobile":"'13825253477",
"receiverName":"袁**",
"remark":"'null",
"salesClassCode":"A",
"source":"TAOBAO",
"status":"CLAIM"
},
{
"actuallyPrice":12,
"appeal":false,
"code":"100078",
"createTime":1513394331000,
"freightPrice":0,
"goodsUrl":"https://h5.youzan.com/v2/showcase/goods?alias=2oe1xck9cn3pp&from=wsc&kdtfrom=wsc",
"orderCreateTime":1511288990000,
"orderTitle":"康复训练球【搭配ober产品】脑 偏瘫 中风康复器材 手指手腕训练",
"picThumbUrl":"https://img.yzcdn.cn/upload_files/2017/11/21/Fmsr76jarWBvJTaibAqraCfOGwEU.gif!120x120.jpg",
"receiverAddress":"湖南省 衡阳市 雁峰区 先锋街道解放路55号摩登百货一楼莱思丽专柜(421****",
"receiverMobile":"'18873413015",
"receiverName":"王**",
"remark":"'null",
"salesClassCode":"A",
"source":"TAOBAO",
"status":"CLAIM"
}
]
```

### 查询所有已领取的订单

> 示例URL：http://127.0.0.1:8081/api/order/listClaimedOrder

> 请求参数：

- doctorCode （医生编码）必输

> Method：POST

> 返回数据示例

```
[{"actuallyPrice":10.00,"appeal":true,"claimTime":1511678805000,"code":"100075","createTime":1511666331000,"doctorCode":"10050","freightPrice":0.00,"orderCreateTime":1511626449000,"orderTitle":"康复训练球【搭配ober产品】脑 偏瘫 中风康复器材 手指手腕训练","receiverAddress":"湖南省 长沙市 其它区 高新区东方红中路589号怡亚通办公楼三楼云医互联(410****","receiverMobile":"'13381092528","receiverName":"王**","remark":"'null","source":"TAOBAO","status":"CLAIMED"},{"actuallyPrice":1648.00,"appeal":false,"claimTime":1512012439000,"code":"100086","createTime":1512012387000,"doctorCode":"10044","freightPrice":0.00,"orderCreateTime":1511611910000,"orderTitle":"全日康电脑中频治疗仪48A-医用级别处方-家人乐享","receiverAddress":"山东省 青岛市 四方区 抚顺支路19号3号楼一单元301室(266****","receiverMobile":"'18153208767","receiverName":"任**","remark":"'null","source":"TAOBAO","status":"CLAIMED"},{"actuallyPrice":464.00,"appeal":false,"claimTime":1511698701000,"code":"100076","createTime":1511666513000,"doctorCode":"10050","freightPrice":0.00,"orderCreateTime":1511528685000,"orderTitle":"粤华褥疮垫QDC-303防褥疮气床垫 褥疮床垫球形球状气垫护理床,康复训练球【搭配ober产品】脑 偏瘫 中风康复器材 手指手腕训练","receiverAddress":"广东省 肇庆市 高要区 南岸街道肇庆碧桂园映月清泉6街19座(526****","receiverMobile":"'13425297141","receiverName":"苏**","remark":"'null","source":"TAOBAO","status":"CLAIMED"}
```

### 认领订单

> 示例URL：http://127.0.0.1:8081/api/order/claimOrder

> 请求参数：  
- orderCode （订单编码）必输
- doctorCode （医生编码）必输

> Method：POST

> 返回数据示例

```
true //认领成功
false //认领失败
```

### 根据订单编码查询订单详情

> 示例URL：http://127.0.0.1:9090/api/order/listDetailByOrderCode

> 请求参数：  
- orderCode （订单编码）必输

> Method：POST

> 返回数据示例

```
[{"code":10017,"count":1,"createTime":1507368694000,"goodsCode":"DZ00003","goodsName":"Ober 康复肩托 偏瘫患者肩关节固定 东正益康复","orderCode":"100025","price":252,"remark":"null","sourceOrderCode":"=\"61697830456905917\""}]
```

### 撤回订单

> 示例URL：http://127.0.0.1:8081/api/order/cancelOrderClaim

> 请求参数：  
- orderCode （订单编码）必输
- doctorCode （医生编码）必输

> Method：POST

> 返回数据示例

```
true 
```

## 账户服务

### 根据医生编码查询帐户信息
    包括帐户余额和支付宝帐户

> 示例URL：http://127.0.0.1:8081/api/account/getAccount

> 请求参数：  
- doctorCode （医生编码）必输

> Method：POST

> 返回数据示例

```
{"alipayModiftyTime":1505559091000,"balance":20.00,"balanceModiftyTime":1505559106000,"code":"10000","createTime":1505559056000,"doctorCode":"10009", "apply":true}
```
> 返回值说明;

- apply 是否允许申请提款,true-可以, false-不可以

###  查询医生预占金额

> 示例URL：http://127.0.0.1:8081/api/account/getAccountOccupyMoney

> 请求参数：  
- doctorCode （医生编码）必输

> Method POST

> 返回数据示例

```
3.30
```

###  更新医生支付宝帐户

> 示例URL：http://127.0.0.1:8081/api/account/updateAlipayAccount

> 请求参数：  
- doctorCode （医生编码）必输
- alipayAccount （支付宝帐户） 必输

> Method POST

> 返回数据示例

```
无
```

###  申请提现

> 示例URL：http://127.0.0.1:8081/api/account/extractBalance

> 请求参数：  
- doctorCode （医生编码）必输

> Method POST

> 返回数据示例

```
无
```

## 短信服务
### 发送验证码
> 示例URL：http://127.0.0.1:8081/api/sms/sendCode

> 请求参数：  
- mobile （手机号码）必输

> Method POST

> 返回数据示例

```
success
```

### 校验验证码
> 示例URL：http://127.0.0.1:8081/api/sms/validateSmsCode

> 请求参数：  
- mobile （医生编码）必输
-smsCode (6位验证码) 必输

> Method POST

> 返回数据示例

```
true | false
```

## 推荐服务
### 通过医生编码查询推荐商品
> 示例URL：http://127.0.0.1:9090/api/recommend/recommendByDoctor

> 请求参数：  
- doctorCode （医生编码）必输

> Method POST

> 返回数据示例

```
[{"buyingPrice":1.00,"code":"1212","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"1","retailPrice":1.00,"supplier":"1","trademark":"1"},{"buyingPrice":1.00,"code":"faf","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"faf","retailPrice":1.00,"supplier":"1","trademark":"1"},{"buyingPrice":1.00,"code":"fafsf","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"11212121","retailPrice":1.00,"supplier":"1","trademark":"1"},{"buyingPrice":1.00,"code":"fdaf","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"fda","retailPrice":1.00,"supplier":"1","trademark":"1"},{"buyingPrice":1.00,"code":"fdafd","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"1","retailPrice":1.00,"supplier":"1","trademark":"1"},{"buyingPrice":1.00,"code":"fdsaf","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"f","retailPrice":1.00,"supplier":"1","trademark":"1"},{"buyingPrice":1.00,"code":"fdsaff","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"faf","retailPrice":1.00,"supplier":"1","trademark":"1"},{"buyingPrice":1.00,"code":"fdsfdsfsf","grossMargins":1.0,"grossProfit":1.00,"manufacturer":"1","name":"1","retailPrice":1.00,"supplier":"1","trademark":"1"}]
```

## 微信相关服务
### 通过CODE查询openid
> 示例URL：http://127.0.0.1:9090/api/weixin/getOpenid

> 请求参数：  
- code （编码）必输

> Method POST

> 返回数据示例

```
s09axvxvfsfwewerwvvvfdsfadsf
```

## 申诉服务
### 发起申诉
> 医生对一笔订单只能同时发起一次申诉 

> 示例url:http://127.0.0.1:9090/api/appeal/add

> 请求参数：  
- doctorCode （医生编码）必输
- orderCode （订单编码）必输

> Method POST

> 返回数据示例

```
true
```
### 检查订单申诉
> 查询医生对此订单是否存在未处理的申诉

> 示例url:http://127.0.0.1:9090/api/appeal/checkOrderAppeal

> 请求参数：  
- doctorCode （医生编码）必输
- orderCode （订单编码）必输

> Method POST

> 返回数据示例

```
true-存在 
false - 不存在
```

## 点评服务
### 增加点评
> 示例URL：http://localhost:9090/api/evaluate/add

> 请求参数：  
- {"goodsCode":"DZ00004", "doctorCode":"10009", "patientUsed":"Y", "applyDisease": "XXX", "applySymptom":"fdsafas", "applyStage":"xxfdsafs", "applyPerson":"fdsafsf", "goodsType":"fdsafsdfa", "remark":"fdsafsafs"}

> 部分字段说明
- goodsCode 商品编码必输 
- doctorCode 医生编码必输

> Method POST

> **Content-Type : application/json**

> 返回数据示例

```
true
```

### 根据医生分页查询点评
> 示例URL：http://localhost:9090/api/evaluate/listByDoctorCode

> 请求参数：  
- doctorCode 医生编码（必输）
- pageNum 页码（非必输，默认为1）
- pageSize 页大小（非必输，默认为10）

> Method POST

> 返回数据示例

```
[
    {
        "applyDisease": "XXX",
        "applyPerson": "fdsafsf",
        "applyStage": "xxfdsafs",
        "applySymptom": "fdsafas",
        "createTime": 1515222592000,
        "doctor": {
            "hospitalName": "中国医学科学院北京市协和医院",
            "name": "李五1",
            "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
            "professionName": "主任医生"
        },
        "doctorCode": "10009",
        "goods": {
            "code": "DZ00305",
            "name": "Ober可调膝关节固定支具下肢支架术后骨折膝盖康复半月板护膝护具",
            "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/11/21/Fo-TLnX0SZPMBjzGe0WFjC4nhfww.jpg!120x120.jpg",
            "retailPrice": 368,
            "sellPoint": "Ober可调膝关节固定支具下肢支架术后骨折膝盖康复半月板护膝护具",
            "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=2ormr5pgc1pct&from=wsc&kdtfrom=wsc"
        },
        "goodsCode": "DZ00305",
        "goodsType": "fdsafsdfa",
        "id": 16,
        "likeCount": 0,
        "patientUsed": "Y",
        "readCount": 0,
        "remark": "fdsafsafs",
        "replyCount": 0,
        "shareCount": 0
    },
    {
        "applyDisease": "XXX",
        "applyPerson": "fdsafsf",
        "applyStage": "xxfdsafs",
        "applySymptom": "fdsafas",
        "createTime": 1515158962000,
        "doctor": {
            "hospitalName": "中国医学科学院北京市协和医院",
            "name": "李五1",
            "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
            "professionName": "主任医生"
        },
        "doctorCode": "10009",
        "goods": {
            "code": "DZ18016",
            "name": "康之乐拉筋板斜板康复器材偏瘫训练器踝关节矫正板康复训练器材",
            "picThumbUrl": "https://img.yzcdn.cn/upload_files/2017/12/26/Fs98NHUFystgKyBB-h4RiMqzDyP7.jpg!120x120.jpg",
            "retailPrice": 228,
            "sellPoint": "康之乐拉筋板斜板康复器材偏瘫训练器踝关节矫正板康复训练器材",
            "sellUrl": "https://h5.youzan.com/v2/showcase/goods?alias=3et074j0d9919&from=wsc&kdtfrom=wsc"
        },
        "goodsCode": "DZ18016",
        "goodsType": "fdsafsdfa",
        "id": 11,
        "likeCount": 0,
        "patientUsed": "Y",
        "readCount": 0,
        "remark": "fdsafsafs",
        "replyCount": 324,
        "shareCount": 0
    }
]
```

> 返回字段说明
- likeCount 喜欢数量
- readCount 阅读量
- replyCount 回复数量
- attentionCount 关注量
- evaluateCount 点评数量
- shareCount 分享数
- portraitImg 医生头像

### 根据商品编码分页查询点评
> 示例URL：http://localhost:9090/api/evaluate/listByGoodsCode

> 请求参数：  
- goodsCode 商品编码必输
- pageNum 页码 (默认为1)
- pageSize 页大小 （默认为10）

> Method POST

> 返回数据示例

```
[
    {
        "applyDisease": "XXX",
        "applyPerson": "fdsafsf",
        "applyStage": "xxfdsafs",
        "applySymptom": "fdsafas",
        "createTime": 1515158959000,
        "doctor": {
            "hospitalName": "中国医学科学院北京市协和医院",
            "name": "李五1",
            "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
            "professionName": "主任医生"
        },
        "doctorCode": "10009",
        "goodsCode": "DZ18016",
        "goodsType": "fdsafsdfa",
        "id": 6,
        "likeCount": 5,
        "patientUsed": "Y",
        "readCount": 6,
        "remark": "fdsafsafs",
        "replyCount": 0,
        "shareCount": 0
    },
    {
        "applyDisease": "Xfdsafxx11XX",
        "applyPerson": "fdsfdsafsf",
        "applyStage": "xxfdsfdsaafs",
        "applySymptom": "fdsfdsafafas",
        "createTime": 1515321516000,
        "doctor": {
            "hospitalName": "中国医学科学院北京市协和医院",
            "name": "张三",
            "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png"
        },
        "doctorCode": "10023",
        "goodsCode": "DZ18016",
        "goodsType": "fdsafsdfdafsfa",
        "id": 22,
        "likeCount": 4,
        "patientUsed": "Y",
        "readCount": 3,
        "remark": "fdsafsafs11212121",
        "replyCount": 2,
        "shareCount": 0
    }
]
```
>部分返回字段说明
- applyDisease 适用的疾病
- applyPerson 适用人员
- applyStage 适用阶段
- applySymptom 适用症状
- patientUsed 是否为患者使用过产品 Y-是， N-否
- goods_type 型号/颜色分类
- remark 描述
- createTime 点评时间

### 增加喜欢
> 示例URL：http://localhost:9090/api/evaluate/addLike

> 请求参数：  
- evaluateId 点评ID（必输）
- likeDoctorCode 点击喜欢按钮的医生编码（必输）

> Method POST

> 返回数据示例

```
true
```

### 取消喜欢
> 示例URL：http://localhost:9090/api/evaluate/cancelLike

> 请求参数：  
- evaluateId 点评ID（必输）
- likeDoctorCode 点击取消喜欢按钮的医生编码（必输）

> Method POST

> 返回数据示例

```
true
```
### 增加阅读数
> 示例URL：http://localhost:9090/api/evaluate/increaseRead

> 请求参数：  
- evaluateId 点评ID（必输）

> Method POST

> 返回数据示例

```
true
```

### 增加分享数
> 示例URL：http://localhost:9090/api/evaluate/increaseShare

> 请求参数：  
- evaluateId 点评ID（必输）

> Method POST

> 返回数据示例

```
true
```

## 关注服务
### 增加关注
> 示例URL：http://localhost:9090/api/attention/addAttention

> 请求参数：  
- doctorCode 医生编码（必输）
- attentionDoctorCode 被关注的医生编码（必输）

> Method POST

> 返回数据示例

```
true
```

### 取消关注
> 示例URL：http://localhost:9090/api/attention/cancelAttention

> 请求参数：  
- doctorCode 医生编码（必输）
- attentionDoctorCode 被取消关注的医生编码（必输）

> Method POST

> 返回数据示例

```
true
```

### 查询医生的关注
> 示例URL：http://localhost:9090/api/attention/listAttention

> 请求参数：  
- doctorCode 医生编码（必输）

> Method POST

> 返回数据示例

```
[
    {
        "attentionDoctorCode": "10018",
        "attentionTime": 1515221519000,
        "doctorCode": "10008",
        "doctorName": "张三",
        "hospitalName": "",
        "id": 17,
        "professionName": "",
        "statistics": {
            "attentionCount": 1,
            "evaluateCount": 12,
            "fansCount": 1,
            "likeCount": 1212,
            "readCount": 1211
        }
    },
    {
        "attentionDoctorCode": "10025",
        "attentionTime": 1515216562000,
        "doctorCode": "10008",
        "doctorName": "1",
        "hospitalName": "卫生部北京医院",
        "id": 9,
        "professionName": "医生/主任",
        "statistics": {
            "attentionCount": 0,
            "evaluateCount": 4,
            "fansCount": 0,
            "likeCount": 12,
            "readCount": 121
        }
    }
]
```

### 查询医生的粉丝
> 示例URL：http://localhost:9090/api/attention/listFans

> 请求参数：  
- doctorCode 医生编码（必输）

> Method POST

> 返回数据示例

```
[{
        "attentionDoctorCode": "10025",
        "attentionTime": 1515217009000,
        "doctorCode": "10018",
        "doctorName": "张三",
        "hospitalName": "",
        "id": 11,
        "professionName": "",
        "statistics": {
            "attentionCount": 1,
            "evaluateCount": 12,
            "fansCount": 1,
            "likeCount": 1212,
            "readCount": 1211
        }
    },
    {
        "attentionDoctorCode": "10025",
        "attentionTime": 1515216986000,
        "doctorCode": "10009",
        "doctorName": "李五1",
        "hospitalName": "中国医学科学院北京市协和医院",
        "id": 10,
        "professionName": "主任医生",
        "statistics": {
            "attentionCount": 1,
            "evaluateCount": 2,
            "fansCount": 0,
            "likeCount": 224,
            "readCount": 1212
        }
}]
```

### 查询医生是否关注其它医生
> 示例URL：http://localhost:9090/api/attention/getAttentionState

> 请求参数：  
- doctorCode 医生编码（必输）
- attentionDoctorCode 关注医生（必输）
> Method POST

> 返回数据示例

```
true - 已关注
false - 未关注
```

## 评论服务
### 增加评论
> 示例URL：http://localhost:9090/api/commentary/add

> 请求参数：  
- doctorCode 医生编码（必输）
- evaluateId 点评ID（必输）
- content 评论内容（最多255个字符）
- parentId 父评论ID （非必输）

> Method POST

> 返回数据示例

```
true
```

### 分页查询点评的评论
> 已按回复时间排序

> 示例URL：http://localhost:9090/api/commentary/listByEvaluateId

> 请求参数：  
- evaluateId 点评ID（必输）
- pageNum 页码 （必输）
- pageSize 页大小 （必输）

> Method POST

> 返回数据示例

```
[
    {
        "content": "大大的赞",
        "createTime": 1515228932000,
        "doctor": {
            "doctorName": "张三",
            "hospitalName": "中国医学科学院北京市协和医院",
            "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
            "professionName": "主任医生"
        },
        "doctorCode": "10008",
        "evaluateId": 5,
        "id": 5
    },
    {
        "content": "大家好，这写的很好",
        "createTime": 1515228223000,
        "doctor": {
            "doctorName": "张三",
            "hospitalName": "中国医学科学院北京市协和医院",
            "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
            "professionName": "主任医生"
        },
        "doctorCode": "10008",
        "evaluateId": 5,
        "id": 2,
        "replyCommentary": [
            {
                "content": "赞一个",
                "createTime": 1515228719000,
                "doctor": {
                    "doctorName": "张三",
                    "hospitalName": "中国医学科学院北京市协和医院",
                    "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
                    "professionName": "主任医生"
                },
                "doctorCode": "10008",
                "evaluateId": 5,
                "id": 3,
                "parentId": 2
            }
        ]
    },
    {
        "content": "大家好，这写的很好",
        "createTime": 1515227446000,
        "doctor": {
            "doctorName": "张三",
            "hospitalName": "",
            "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
            "professionName": ""
        },
        "doctorCode": "10018",
        "evaluateId": 5,
        "id": 1,
        "replyCommentary": [
            {
                "content": "赞2个",
                "createTime": 1515228909000,
                "doctor": {
                    "doctorName": "张三",
                    "hospitalName": "",
                    "professionName": ""
                },
                "doctorCode": "10008",
                "evaluateId": 5,
                "id": 4,
                "parentId": 1
            }
        ]
    }
]
```
>部分返回字段说明
- content 评论内容
- doctor 评论人信息
- replyCommentary 回复此评论集合

### 查询评论的所有回复
> 已按回复时间排序，第一层回复分页，其它层回复查询所有

> 示例URL：http://localhost:9090/api/commentary/listByParentId

> 请求参数：  
- parentId 评论ID（必输）
- pageNum 页码 （必输）
- pageSize 页大小 （必输）

> Method POST

> 返回数据示例

```
[
    {
        "content": "第二层赞",
        "createTime": 1515228719000,
        "doctorCode": "10008",
        "evaluateId": 5,
        "id": 3,
        "parentId": 2,
        "replyCommentary": [
            {
                "content": "第三层赞",
                "createTime": 1515230467000,
                "doctor": {
                    "doctorName": "张三",
                    "hospitalName": "中国医学科学院北京市协和医院",
                    "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
                    "professionName": "主任医生"
                },
                "doctorCode": "10008",
                "evaluateId": 5,
                "id": 6,
                "parentId": 3,
                "replyCommentary": [
                    {
                        "content": "第四层赞",
                        "createTime": 1515230503000,
                        "doctor": {
                            "doctorName": "张三",
                            "hospitalName": "中国医学科学院北京市协和医院",
                            "professionName": "主任医生"
                        },
                        "doctorCode": "10008",
                        "evaluateId": 5,
                        "id": 7,
                        "parentId": 6
                    }
                ]
            }
        ]
    },
    {
        "content": "第二1层赞",
        "createTime": 1515231044000,
        "doctorCode": "10008",
        "evaluateId": 5,
        "id": 8,
        "parentId": 2,
        "replyCommentary": [
            {
                "content": "第二211层赞",
                "createTime": 1515231258000,
                "doctor": {
                    "doctorName": "张三",
                    "hospitalName": "中国医学科学院北京市协和医院",
                    "portraitImg": "http://rehab.dongzhengyi.com/img/portrait/1.png",
                    "professionName": "主任医生"
                },
                "doctorCode": "10008",
                "evaluateId": 5,
                "id": 10,
                "parentId": 8
            }
        ]
    },
    {
        "content": "第二2层赞",
        "createTime": 1515231169000,
        "doctorCode": "10008",
        "evaluateId": 5,
        "id": 9,
        "parentId": 2
    }
]
```