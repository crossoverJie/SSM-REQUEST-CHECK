

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

