# 预售系统服务端接口文档
### 前置说明：

1、所有请求全部统一采用post方式，所有参数都以json的方式发送，所有的结果都以json的方式返回

2、所有的返回json都为如下格式：

	{
  		"code": 返回码,
  		"msg": "返回信息",
  		"res": 返回内容
	}
	
	说明：code值为0，代表正确返回，如果非0，代表有错误，msg为错误信息

3、所有图片若没有特殊要求，一律采用png的格式

### 相关接口：
##### 获取主页信息
###### 请求url:
	/api/commodity/index
	
###### 请求参数
	{
  		"commodity_id": 1           #商品id
	}

###### 返回
	{
    "msg":"success",
    "res":{
        "recommends":[
            {
                "profile":"http://wx.qlogo.cn/mmopen/Q3auHgzwzM5P3VAUb4kc3XPjU2B7IdIuDECTQMTFY5vQRojBJT3rg0poXpbs29jFlk1pibE8pjGnYpZBYLMwbYBEkicxWORHJRSNSwWdLDANY/0",
                "content":"衣服真好看，太实惠了",
                "job":"服务端开发工程师",
                "name":"郭广川",
                "is_v":1
            },
            {
                "profile":"http://wx.qlogo.cn/mmopen/ajNVdqHZLLD7p3thFMGrGd2xNPMeFcNSA74OfMs9O3jTr51Qf3EiaY46lSNujZNzqXibmQsLg4OlbjJXRicyOlntg/0",
                "content":"衣服真好看，太实惠了",
                "job":"财务经理",
                "name":"张敏",
                "is_v":1
            },
            {
                "profile":"http://wx.qlogo.cn/mmopen/anblvjPKYbOxTuZdQ8Ziae8Dia7JUia74l4Mx5dJEoBcjS77LcoKQekrricVQVhaPjfGM62ttWnt3pemibBUaWFJkKozGFVE99Frq/0",
                "content":"衣服真好看，太实惠了",
                "job":"360产品经理",
                "name":"刘怡君",
                "is_v":1
            }
        ],
        "cart_count":5,
        "presell_count":100,
        "logo":"/instatic/preseller/img/publisher/1/logo.png",
        "publisher_name":"GSTEPS-街舞社",
        "title":"G-STEPS卫衣预售",
        "brief":"360程序员做的衣服好看又耐穿",
        "satisfy_count":80,
        "commodity_id":"1",
        "sold_count":50,
        "navigation":"/instatic/preseller/img/commodity/1/navigation.png"
    },
    "code":0
	}

##### 获取商品属性，及对应属性价格和图片
###### 请求url:
		
	/api/commodity/attribute
###### 请求参数：

	{
  		"commodity_id": 1,              #商品id， 必填项
  		"selected_options_ids": [1,4,7],  #选中的属性选项id列表， 可选项
  		"count":5                       #选择商品数量， 可选项
	}
	
	说明：
	1、属性：例如衣服的颜色color，可以称为衣服的属性
	2、属性选项：例如颜色中的黑色black，可以称为属性选项
	3、若select_options_ids这个key不存在，那么使用该属性的默认选项
	4、若count这key不存在，那么count取值1
	5、价格计算：每种商品有一个base_price，每个属性选项有一个价格权重值weight，那么购物车中该商品的总价格为(设定有三种属性)：price = (base_price + weight1 + weight2 + weight3) * count
	
###### 返回
	{
    "msg":"success",
    "res":{
        "count":5,                               #原样返回商品数量
        "sample_img":"/instatic/preseller/img/commodity/1/attribute/black.jpg"                                       #样图
        "price": 530,                             #总价格
        "commodity_id":1,                        #原样返回商品id
        "title":"G-STEPS卫衣预售",                 #商品标题
        "base_price":100,                        #商品基价
        "attributes":[
            {
                "index":1,                       #前端属性展示顺序
                "cn_attr_name":"颜色",            #展示属性名
                "attr_name":"color",             #英文属性名
                "options":[
                    {
                        "index":1,              #前端属性选项展示顺序
                        "weight":0,             #属性选项价格权重
                        "option_name":"black",  #英文属性选项名 
                        "option_id":1,          #属性选项id
                        "default":false,        #非默认选项
                        "cn_option_name":"黑色",#展示属性选项名 
                        "select":true           #被用户选择 
                    },
                    {
                        "index":2,
                        "weight":0,
                        "option_name":"red",
                        "option_id":2,
                        "default":true,         #默认选项
                        "cn_option_name":"红色",
                        "select":false          #没有被用户选择
                    },
                    {
                        "index":3,
                        "weight":3,
                        "option_name":"pink",
                        "option_id":3,
                        "default":false,         #非默认选项
                        "cn_option_name":"粉红色",
                        "select":false          #没有被用户选择
                    }
                ]
            },
            {
                "index":2,
                "cn_attr_name":"大小",
                "attr_name":"size",
                "options":[
                    {
                        "index":1,
                        "weight":1,
                        "option_name":"m",
                        "option_id":4,
                        "default":false,
                        "cn_option_name":"M",
                        "select":true
                    },
                    {
                        "index":2,
                        "weight":4,
                        "option_name":"l",
                        "option_id":5,
                        "default":true,
                        "cn_option_name":"L",
                        "select":false
                    },
                    {
                        "index":3,
                        "weight":8,
                        "option_name":"xl",
                        "option_id":6,
                        "default":false,
                        "cn_option_name":"XL",
                        "select":false
                    }
                ]
            },
            {
                "index":3,
                "cn_attr_name":"材质",
                "attr_name":"texture",
                "options":[
                    {
                        "index":1,
                        "weight":5,
                        "option_name":"cotton",
                        "option_id":7,
                        "default":true,
                        "cn_option_name":"棉",
                        "select":true
                    },
                    {
                        "index":2,
                        "weight":0,
                        "option_name":"silk",
                        "option_id":8,
                        "default":false,
                        "cn_option_name":"丝绸",
                        "select":false
                    }
                ]
            }
        ]
    },
    "code":0
	}
