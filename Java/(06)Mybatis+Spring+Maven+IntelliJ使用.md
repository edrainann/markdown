# (06)Mybatis+Spring+Maven+IntelliJ使用

### 一、创建一个新的Spring项目

####  1、打开IntelliJ -> 文件 -> New -> 项目 -> 选择 Spring Initializr

![image.png](https://upload-images.jianshu.io/upload_images/1683050-84d5bc96e5c84939.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/1683050-0a00438bb32baa96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/1683050-b9bbcffbf10a1163.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/1683050-df38445517677a1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择完了之后，就耐心等待加载出来。

默认生成的目录格式是这样的：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-cbae6268f24a3842.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2、在pom.xml中加入 安装mybatis-generator插件

```xml
		 <plugin>
               <groupId>org.mybatis.generator</groupId>
               <artifactId>mybatis-generator-maven-plugin</artifactId>
               <version>1.3.7</version>
                <configuration>
                   <verbose>true</verbose>
                   <overwrite>true</overwrite>
                </configuration>
            </plugin>
```

![image.png](https://upload-images.jianshu.io/upload_images/1683050-b75e520b23844d8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3、安装Lombok

```xml
  	   <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```

避免后面用到builder会变红的问题。

### 二、配置resources文件

#### 1、路径：src—》main—》java—》resources  

添加 **generatorConfig.xml**文件

```xml
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--数据库驱动 -->
    <classPathEntry location="E:\repository\mysql\mysql-connector-java\5.1.46\mysql-connector-java-5.1.46.jar"/>
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <property name="autoDelimitKeywords" value="true"/>
        <!-- beginningDelimiter和endingDelimiter：指明数据库的用于标记数据库对象名的符号，比如ORACLE就是双引号，MYSQL默认是`反引号； -->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        <!--数据库链接地址账号密码 -->
        <!-- 本地 环境 数据库链接地址账号密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306"
                        userId="edrain" password="12345abcde">
            <!-- 针对mysql数据库 -->
            <property name="useInformationSchema" value="true"/>
        </jdbcConnection>
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>
        <!--生成Model类存放位置 -->
        <javaModelGenerator targetPackage="com.edrain.mybatis.entity" targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <!--生成映射文件存放位置 -->
        <sqlMapGenerator targetPackage="mybatis" targetProject="src/main/resources">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>
        <!--生成Dao类存放位置 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.edrain.mybatis.mapper" targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>
        <!--生成对应表及类名 -->
        <table tableName="sc_white_list" domainObjectName="WhiteList" enableCountByExample="true"
               enableUpdateByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
               selectByExampleQueryId="true"/>
    </context>

</generatorConfiguration>
```

#### 2、执行 **mybatis-generator**  后 -》  自动生成 

src-》main-》java-》com.edrain.mybatis-》entity：WhiteList 、WhiteListExample

src-》main-》java-》com.edrain.mybatis-》mapper：WhiteListMapper

src-》main-》resources -》WhiteListMapper.xml



![image.png](https://upload-images.jianshu.io/upload_images/1683050-637ee58b9aa45b78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三、运行主内容

#### 1、编写主文件

路径：src--》test--》java--》com.edrain.mybatis --》Auth.java

```java
package com.edrain.mybatis;

import com.edrain.mybatis.entity.WhiteList;
import com.edrain.mybatis.mapper.WhiteListMapper;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import javax.annotation.Resource;
import java.util.UUID;


@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class Auth extends TestBase {

//    @Autowired  //Spring注解
    @Resource //它的包是javax.annotation.Resource
    private WhiteListMapper whiteListMapper;

    /**生成随机身份证号码
     */
    @Test
    public void idCardNumber() {
        JavaTools cre = new JavaTools();
        String randomID = cre.getRandomID();
        System.out.println("生成的身份证号码是：" + randomID);
        String phone = phoneMaker();  //生成电话号码
        System.out.println("生成的电话号码是：" + phone);
    }

    /**在数据库白名单没有插入
     *手机号码，身份证
     */
    @Test
    public void insert19SqlNoPhoneIdcardAuth() throws Exception {
        String idCardNumber = idCardNumber01(); //生成身份证号码
        String phone = phoneMaker();  //生成电话号码
        int threeNumber = threeNumber();  //生成三位数
        WhiteList x = WhiteList.builder()
                .code(UUID.randomUUID().toString().replaceAll("-", ""))
                .thirdUserNo(String.valueOf(threeNumber))
                .productNo("25G")
                .merchantId(4L)
                .merchantName("人和易行哈")
                .isDeleted((byte) 0)
                .build();
        whiteListMapper.insertSelective(x);
        System.out.println("添加的手机号码是：" + phone);
        System.out.println("成功添加");
    }
}
```



#### 2、添加注释

src-》main-》java-》com.edrain.mybatis-》entity：WhiteList.java 中

```java
@Builder
@NoArgsConstructor
@ToString
@AllArgsConstructor
```

![image.png](https://upload-images.jianshu.io/upload_images/1683050-19bf3ce7a552c059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



src-》main-》java-》com.edrain.mybatis-》mapper：WhiteListMapper.java 中

```
@Mapper
```

![image.png](https://upload-images.jianshu.io/upload_images/1683050-4db0bbbd3fceb149.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3、配置application.properties

路径：src—》main  —》resources —》application.properties

```properties
spring.datasource.url=jdbc:mysql://127.0.0.1:3306?useUnicode=true&characterEncoding=UTF8&zeroDateTimeBehavior=convertToNull&useSSL=false
spring.datasource.username=edrain
spring.datasource.password=12345abcde
mybatis.mapper-locations=classpath*:/mybatis/**Mapper.xml
logging.level.cn.xyd.supply.test.mapper=debug
```

#### 4、pom.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.xyd.supply</groupId>
    <artifactId>test</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>test</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
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

    <build>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.7</version>
                <configuration>
                    <verbose>true</verbose>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

