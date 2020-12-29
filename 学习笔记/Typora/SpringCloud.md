# SpringCloud

## 一、SpringCloud概述

==微服务架构是一种架构模式，提倡将单一应用程序划分成一组小的服务，服务之间相互协调、互相配合，每个服务运行在独立的进程，并且能够被独立的部署到生产环境、类生产环境等，总结来说就是多个模块化的小型服务的协调和配合。==

![image-20201224074726758](../Typora图片/SpringCloud/image-20201224074726758.png)

**SpringCloud：**分布式微服务架构的一站式解决方案，==是多种微服务架构落地技术的集合体==，俗称微服务全家桶。![image-20201224075106726](../Typora图片/SpringCloud/image-20201224075106726.png)

**springcloud和springboot版本对应：**https://start.spring.io/actuator/info

## 二、SpringCloud技术升级

![image-20201224081936138](../Typora图片/SpringCloud/image-20201224081936138.png)

参考文档：

- https://spring.io/projects/spring-cloud
- https://www.bookstack.cn/read/spring-cloud-docs/docs-project-SpringCloudCloudFoundryServiceBroker.md

## 三、项目构建

### 3.1、父工程POM

````xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.tanx.cloud</groupId>
    <artifactId>Cloud-Test</artifactId>
    <version>1.0-version</version>
    <packaging>pom</packaging>

    <modules>
        <module>Provider</module>
    </modules>

    <!-- 统一管理jar包版本 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.18.12</lombok.version>
        <mysql.version>8.0.19</mysql.version>
        <druid.version>1.1.23</druid.version>
        <druid.spring.boot.starter.version>1.1.10</druid.spring.boot.starter.version>
        <spring.boot.version>2.2.2.RELEASE</spring.boot.version>
        <spring.cloud.version>Hoxton.SR1</spring.cloud.version>
        <spring.cloud.alibaba.version>2.1.0.RELEASE</spring.cloud.alibaba.version>
        <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
        <mybatis-spring-boot-starter.version>2.1.2</mybatis-spring-boot-starter.version>
        <hutool-all.version>5.1.0</hutool-all.version>
        <spring.cloud.alibaba.version>2.1.0.RELEASE</spring.cloud.alibaba.version>
    </properties>

    <distributionManagement>
        <site>
            <id>website</id>
            <url>scp://webhost.company.com/www/website</url>
        </site>
    </distributionManagement>

    <!-- 子模块继承之后，提供作用：锁定版本 + 子module不用谢groupId和version -->
    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring.boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--Spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring.cloud.alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>${druid.spring.boot.starter.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis-spring-boot-starter.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
````

### 3.2、Provider

**POM：**

````xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--监控-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <!--如果没写版本,从父层面找,找到了就直接用,全局统一-->
    </dependency>
    <!--mysql-connector-java-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <!--jdbc-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <!--热部署-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
````

**mapper：**

````xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.tanx.cloud.dao.ProviderDao">
    <insert id="add" parameterType="com.tanx.cloud.pojo.Provider" useGeneratedKeys="true" keyProperty="id">
        insert into `order`(serial) values (#{serial});
    </insert>

    <select id="getProviderById" resultMap="BaseResultMapper" parameterType="Long">
        select * from `order` where id = #{id}
    </select>

    <resultMap id="BaseResultMapper" type="com.tanx.cloud.pojo.Provider">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <result column="serial" property="serial" jdbcType="VARCHAR"/>
    </resultMap>
</mapper>
````

**CommonResult：**返回公共类

````
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T> implements Serializable {

    private Integer code;
    private String  message;
    private T       data;

    public CommonResult(Integer code,String message){
        this(code,message,null);
    }
}
````

**Provider：**

````JAVA
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Provider implements Serializable {
    private Long   id;
    private String serial;
}
````

**Controller层：**

````java
@PostMapping(value = "/provider/add")
public CommonResult<Object> add(Provider provider){
    int result = providerService.add(provider);
    log.info("*******插入结果："+result);
    return result > 0 ? new CommonResult<>(200, "插入成功", result):new CommonResult<>(500,"插入失败",null);
}

@GetMapping(value = "/provider/query/{id}")
public CommonResult<Object> getProviderById(@PathVariable("id") Long id){
    Provider provider = providerService.getProviderById(id);
    log.info("*******查询结果："+provider);
    return provider != null ? new CommonResult<>(200, "查询成功",provider):new CommonResult<>(500,"查询失败",null);
}
````

**add：**

![image-20201224132949269](../Typora图片/SpringCloud/image-20201224132949269.png)

**getProviderById：**

![image-20201224133010211](../Typora图片/SpringCloud/image-20201224133010211.png)

### 3.3、Consumer

**POM：**

````XML
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
</dependencies>
````

**Provider：**

````java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Provider {
    private Long id;
    private String serial;
}
````

**CommonResult：**返回公共类

````java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T> implements Serializable {

    private Integer code;
    private String  message;
    private T       data;

    public CommonResult(Integer code,String message){
        this(code,message,null);
    }
}
````

**RestTemplateConfig：**

````java
@Configuration
public class RestTemplateConfig {

    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();  // 类似HttpClient,可以通过网络请求获取数据
    }
}
````

**ConsumerController：**

```java
@RestController
@SuppressWarnings("unchecked")
public class ConsumerController {

    private final static String URL = "http://localhost:8001";

    @Resource
    private RestTemplate restTemplate;

    @PostMapping(value = "/consumer/provider/add")
    public CommonResult<Provider> add(@RequestBody Provider provider){
        return restTemplate.postForObject(URL+"/provider/add",provider,CommonResult.class);
    }

    @GetMapping(value = "/consumer/provider/query/{id}")
    public CommonResult<Provider> getProviderById(@PathVariable("id") Long id){
        return restTemplate.getForObject(URL + "/provider/query/" + id, CommonResult.class);
    }
}
```

**add：**

![image-20201224185028738](../Typora图片/SpringCloud/image-20201224185028738.png)

**getProviderById：**

![image-20201224183958500](../Typora图片/SpringCloud/image-20201224183958500.png)