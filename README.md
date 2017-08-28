[![Build Status](https://travis-ci.org/crossoverJie/SSM-REQUEST-CHECK.svg?branch=master)](https://travis-ci.org/crossoverJie/SSM-REQUEST-CHECK)

# 开发
https://crossoverjie.top/2017/05/24/SSM14/

# 预览
![重复请求号01.jpg](https://ooo.0o0.ooo/2017/05/24/59245e69e35d5.jpg)
![重复请求号02.jpg](https://ooo.0o0.ooo/2017/05/24/59245e6a643ea.jpg)
![重复请求号03.jpg](https://ooo.0o0.ooo/2017/05/24/59245e6b55994.jpg)


# 简介
基于annotation的http去重插件：

- `Redis`保存请求。
- `Spring AOP` 进行切面。

**后续会用`Spring Boot`重构**

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
    <version>1.0.0</version>
</dependency>
```

## 开启CGLIB代理

> 需要根据自己`web.xml`中定义的
```xml
    <!-- Spring MVC servlet -->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
        <async-supported>true</async-supported>
    </servlet>
```
`spring-mvc.xml`文件中配置：


```xml
<aop:aspectj-autoproxy proxy-target-class="true"></aop:aspectj-autoproxy>
```

## 使用注解
```java
    @CheckReqNo
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

## 自定义缓存前缀、时间
> 默认缓存前缀是`reqNo`,时间为1天。

```
#redis前缀
redis.prefixReq=reqNo
#redis缓存时间 默认单位为天
redis.day=1
```

# 联系作者
- [crossoverJie@gmail.com](mailto:crossoverJie@gmail.com)

![weixinchat.jpg](https://ooo.0o0.ooo/2017/04/25/58ff4d5fd9fa3.jpg)
