### 集成Security和Oauth2
修改api-gatewway的pom.xml:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

	<modelVersion>4.0.0</modelVersion>

	<artifactId>microservice-api-gateway</artifactId>
	<packaging>jar</packaging>

	<parent>
		<groupId>com.itmuch.cloud</groupId>
		<artifactId>spring-cloud-microservice-study</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>

	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-zuul</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-security</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-oauth2</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
	</dependencies>
</project>
```
启动类：

```java
/**
 * 使用@EnableZuulProxy注解激活zuul，同时跟进入该注解可以看到该注解整合了@EnableCircuitBreaker、@EnableDiscoveryClient，是个组合注解，目的是简化配置。
 * @author eacdy
 */
@SpringBootApplication
@EnableZuulProxy
@EnableResourceServer #开启资源服务器
@EnableAuthorizationServer #开启oauth授权服务器
public class ZuulApiGatewayApplication {
	public static void main(String[] args) {
		SpringApplication.run(ZuulApiGatewayApplication.class, args);
	}
}
```

配置文件：application.yml

```yaml
spring:
  application:
    name: microservice-api-gateway
server:
  port: 8050
  
security:
  user:
    name: user
    password: password
    role:
      - user
      - admin
  oauth2:
    client:
      clientId: bd1c0a783ccdd1c9b9e4
      clientSecret: 1a9030fbca47a5b2c28e92f19050bb77824b5ad1
    resource:
      userInfoUri: https://localhost:8050/user
      preferTokenInfo: false    
  
eureka:
  instance:
    hostname: gateway
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
```

这样，spring-cloud-security和spring-cloud-oauth2就集成成功。



### 测试

第一步：启动microservice-api-gateway项目。
第二步：获取授权token
```shell
curl bd1c0a783ccdd1c9b9e4:1a9030fbca47a5b2c28e92f19050bb77824b5ad1@localhost:8050/oauth/token -d grant_type=password -d username=user -d password=password -d scope=openid
```

```json
{"access_token":"22f5f196-4878-4b79-94c6-b86961c8a5a0","token_type":"bearer","refresh_token":"fddc2fe9-58d1-4262-916c-7ea366132498","expires_in":43199,"scope":"openid"}
```
第三步：使用上一步获取的access_token请求API接口：
```shell
curl http://localhost:8050/movie/ribbon/1?access_token=22f5f196-4878-4b79-94c6-b86961c8a5a0
```

```json
{"id":1,"username":"Tom","age":12}
```
	如果不带access_token作为参数请求：
```shell
curl http://localhost:8050/movie/ribbon/1
```
```json
{"error":"unauthorized","error_description":"Full authentication is required to access this resource"}
```

ps：时间有限，后面有时间会实现：
1. 实现client和user持久化
2. access_token缓存到redis
3. token使用jwt加密
4. api接口限流
5. 。。。
