## 第1节：集成Security和Oauth2

编辑api-gateway的pom.xml文件，修改为：

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;project xmlns="http:\/\/maven.apache.org\/POM\/4.0.0" xmlns:xsi="http:\/\/www.w3.org\/2001\/XMLSchema-instance"

 xsi:schemaLocation="http:\/\/maven.apache.org\/POM\/4.0.0 http:\/\/maven.apache.org\/xsd\/maven-4.0.0.xsd"&gt;





 &lt;modelVersion&gt;4.0.0&lt;\/modelVersion&gt;





 &lt;artifactId&gt;microservice-api-gateway&lt;\/artifactId&gt;

 &lt;packaging&gt;jar&lt;\/packaging&gt;





 &lt;parent&gt;

 &lt;groupId&gt;com.itmuch.cloud&lt;\/groupId&gt;

 &lt;artifactId&gt;spring-cloud-microservice-study&lt;\/artifactId&gt;

 &lt;version&gt;0.0.1-SNAPSHOT&lt;\/version&gt;

 &lt;\/parent&gt;





 &lt;dependencies&gt;

 &lt;dependency&gt;

 &lt;groupId&gt;org.springframework.cloud&lt;\/groupId&gt;

 &lt;artifactId&gt;spring-cloud-starter-zuul&lt;\/artifactId&gt;

 &lt;\/dependency&gt;

 &lt;dependency&gt;

 &lt;groupId&gt;org.springframework.cloud&lt;\/groupId&gt;

 &lt;artifactId&gt;spring-cloud-starter-security&lt;\/artifactId&gt;

 &lt;\/dependency&gt;

 &lt;dependency&gt;

 &lt;groupId&gt;org.springframework.cloud&lt;\/groupId&gt;

 &lt;artifactId&gt;spring-cloud-starter-oauth2&lt;\/artifactId&gt;

 &lt;\/dependency&gt;

 &lt;dependency&gt;

 &lt;groupId&gt;org.springframework.cloud&lt;\/groupId&gt;

 &lt;artifactId&gt;spring-cloud-starter-eureka&lt;\/artifactId&gt;

 &lt;\/dependency&gt;

 &lt;\/dependencies&gt;

&lt;\/project&gt;

