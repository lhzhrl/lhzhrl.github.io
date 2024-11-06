

## 概述

turi-api-sdk 是由途易官方提供的java sdk，方便调试使用。

turi-api-sdk 集成了实时查询、面单相关、寄件等接口。

## 特性

- 提供了turi-api接口请求参数实体类、返回实体类。
- 提供测试类调试。
- 支持maven引入

## 开始集成

Maven
```
<dependency>
	<groupId>com.turi.api</groupId>
	<artifactId>turi-api-sdk</artifactId>
	<version>1.0.1</version>
</dependency>
```

<span id="shipcreate"></span>
### 寄件下单接口
``` java
CreateShipPackageReq createShipPackageReq = objectMapper.readValue(CreateShipPackageReq.json, CreateShipPackageReq.class);  
  
BaseClientReq baseClientReq = new BaseClientReq();  
baseClientReq.setMethod("create");  
baseClientReq.setParam(JSON.toJSONString(createShipPackageReq));  
  
IBaseClient baseClient = new ShipClient("http://192.168.150.242:80");  
HttpResult result = baseClient.execute(baseClientReq);  
if (result.getStatus()== 200){  
    String body = result.getBody();  
    System.out.println("创建单面成功=====>" + body);  
    CreateShipResp createShipResp = JSON.parseObject(body, CreateShipResp.class);  
    System.out.println("单面追踪号=====>" + createShipResp.getMasterTrackingNumber());  
} else {  
    System.out.println("创建单面失败=====>" + result.getError());  
}
```


<span id="shipcancel"></span>
### 寄件取消接口
``` java
CancelShipPackageReq cancelShipPackageReq = objectMapper.readValue(CancelShipPackageReq.json, CancelShipPackageReq.class);  
  
BaseClientReq baseClientReq = new BaseClientReq();  
baseClientReq.setMethod("cancel");  
baseClientReq.setParam(JSON.toJSONString(cancelShipPackageReq));  
  
IBaseClient baseClient = new ShipClient(endpoint);  
HttpResult result = baseClient.execute(baseClientReq);  
if (result.getStatus()== 200){  
    String body = result.getBody();  
    CancelShipResp cancelShipResp = JSON.parseObject(body, CancelShipResp.class);  
    System.out.println("取消单面成功=====>" + cancelShipResp.isCancelledShipment());  
} else {  
    System.out.println("取消单面失败=====>" + result.getError());  
}
```



<span id="trackingnumbers"></span>
### 追踪接口
``` java
TrackingnumbersPackageReq trackingnumbersPackageReq = objectMapper.readValue(TrackingnumbersPackageReq.json, TrackingnumbersPackageReq.class);  
  
BaseClientReq baseClientReq = new BaseClientReq();  
baseClientReq.setMethod("trackingnumbers");  
baseClientReq.setParam(JSON.toJSONString(trackingnumbersPackageReq));  
  
IBaseClient baseClient = new TrackClient(endpoint);  
HttpResult result = baseClient.execute(baseClientReq);  
System.out.println(result);  
if (result.getStatus()== 200){  
    String body = result.getBody();  
    System.out.println("追踪成功=====>" + body);  
    List<TrackResp> trackRespList = JSON.parseArray(body, TrackResp.class);  
    System.out.println(trackRespList);  
} else {  
    System.out.println("追踪失败=====>" + result.getError());  
}
```

