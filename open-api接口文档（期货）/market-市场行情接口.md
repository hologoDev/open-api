### url:openapi.hologo.io

## 接口地址:api/market

## 接口说明:(post请求)市场行情


|参数|	填写类型|	说明|
|------------|--------|-----------------------------|
|contractId|	必填|	合约ID|
|api_key|	必填|	api_key|
|time|	必填|	时间戳|
|sign|	必填|	签名|

返回值:

	{
	    "code":"0",
	    "data":{
	        "high":3555.18, //24h最高价
	        "vol":17, //24h成交量
	        "indexPrice":3437.44, //指数价格
	        "last":3450.29, //最新成交价
	        "low":20, //24h最低价
	        "buy":"3450.290", //买一
	        "sell":"3555.150", //卖一
	        "rose":-0.02949523, //涨跌幅
	        "time":1548658188056, //时间戳
	        "tagPrice":3437.74 //标价价格
	    },
	    "msg":""
	}
