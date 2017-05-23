

# 简介
基于annotation的http去重插件：

- redis保存请求。
- Spring AOP 进行切面。


# 安装
```
https://github.com/crossoverJie/SSM-REQUEST-CHECK.git
```

```
cd SSM-REQUEST-CHECK
```

```
mvn clean
```

```
mvn install
```


# 使用

## 加入依赖

```xml
<dependency>
    <groupId>com.crossoverJie</groupId>
    <artifactId>SSM-REQUEST-CHECK</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

## 开启CGLIB代理

```xml
<aop:aspectj-autoproxy proxy-target-class="true"></aop:aspectj-autoproxy>
```

## 使用注解
```java
    @ReqNoDesc
    @RequestMapping(value = "/createRedisContent",method = RequestMethod.POST)
    @ResponseBody
    public BaseResponse<NULLBody> createRedisContent(@RequestBody RedisContentReq redisContentReq){
        BaseResponse<NULLBody> response = new BaseResponse<NULLBody>() ;

        Rediscontent rediscontent = new Rediscontent() ;
        try {
            CommonUtil.setLogValueModelToModel(redisContentReq,rediscontent);
            rediscontentMapper.insertSelective(rediscontent) ;
            response.setReqNo(redisContentReq.getReqNo());
            response.setCode(StatusEnum.SUCCESS.getCode());
            response.setMessage(StatusEnum.SUCCESS.getMessage());
        }catch (Exception e){
            logger.error("system error",e);
            response.setReqNo(response.getReqNo());
            response.setCode(StatusEnum.FAIL.getCode());
            response.setMessage(StatusEnum.FAIL.getMessage());
        }

        return response ;

    }
```

