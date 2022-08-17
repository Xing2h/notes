# 一、SpringBoot2核心技术-基础

## 01、Spring与SpringBoot

## 02、SpringBoot2入门

### 2、创建HelloWorld程序

需求：发送/hello请求，相应 Hello，SpringBoot 2！

#### 2.1、创建Maven工程

#### 2.2、导入依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.7.1</version>
</parent>

<dependencies>
    <!--web场景启动器-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

#### 2.3、创建主程序

~~~Java
/**
 * 主程序类
 *
 * @SpringBootApplication ： 这是一个SpringBoot应用
 */
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }
}
~~~

#### 2.4、编写业务

```Java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(){
        return "Hello，SpringBoot 2！";
    }
}
```

#### 2.5、测试

直接运行main方法

**注意：**==主程序必须在controller目录同级，不然运行不出结果==

#### 2.6、简化配置

在resources目录下编写application.properties配置文件

```XML
server.port=8888//修改端口为8888
```

#### 2.7、简化部署

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

把项目打成jar包，直接在目标服务器执行即可

## 03、了解自动配置原理

### 1、SpringBoot特点

#### 1.1、依赖管理

- 父项目做依赖管理

  引入父项目的话，当前项目会自动引用父项目的依赖

  ```XML
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.7.1</version>
  </parent>
  他的父项目
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>2.7.1</version>
  </parent>
  
  几乎声明了所有开发中常用的依赖的版本号，自动版本仲裁机制(即dependence里面不用写版本，会自动确定)
  ```

- 开发导入starter场景启动器

  ```xml
  1、可以见到很多 spring-boot-starter-'*' *代表场景
  2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
  3、SpringBoot所有支持场景
  https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.starters
  4、自定义starter	'*'-spring-boot-starter:第三方为我们提供的简化开发的场景启动器
  5、所有场景启动器最底层的依赖都会引用
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.7.1</version>
      <scope>compile</scope>
  </dependency>
  ```

- 无需关注版本号，自动版本仲裁

  ```XML
  1、引入依赖默认都可以不写版本号
  2、引入非版本仲裁的jar，需要自己写版本号
  ```

- 可以修改版本号

  ```XML
  1、查看spring-boot-dependencies里面规定当前依赖的版本 用的key
  2、在当前项目里面重写配置
  <properties>
  	<mysql.version>5.1.53</mysql.version>
  </properties>
  ```


#### 1.2、自动配置

-   自动配好Tomcat

    -   引入tomcat依赖
    -   配置tomcat

    ```XML
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <version>2.7.1</version>
        <scope>compile</scope>
    </dependency>
    ```

-   自动配好SpringMVC

    -   引入SpringMVC全套组件
    -   自动配好SpringMVC（功能）

    ```XML
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.21</version>
        <scope>compile</scope>
    </dependency>
    ```

-   自动配好web常见的功能，如：字符编码问题

    -   SpringBoot帮我们配置好了所有web开发常见场景

-   默认的的包结构

    -   主程序所在包机器下面的所有子包里面的组件都会被默认扫描进来

    -   无需以前的包扫描配置

    -   想要改变扫描路径，==@SpringBootApplication(scanBasePackages = "com.xing")==

        -   或者@ComponentScan指定扫描路径

        -   ```
            @SpringBootApplication
            等同于
            @SpringBootConfiguration
            @EnableAutoConfiguration
            @ComponentScan("com.xing")
            ```

            

-   各种配置拥有默认值

    -   默认配置最终都是映射到某一个类上
    -   配置文件的值最终会绑定每个类上，这个类会在容器中创建对象

-   按需加载所有的自动配置项

    -   有非常多的starter
    -   引入了那个场景的starter，这个场景的配置才会自动开启

### 2、容器功能

#### 2.1、组件添加

**1、@Configuration**

-   基本使用