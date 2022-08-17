

# 0.Spring5

## 0.1、概述

1. Spring是一个轻量级的开源的JavaEE框架。

2. Spring可以解决企业应用开发的复杂性。

3. Spring的两个核心部分：IOC和AOP。

   1. IOC：控制反转，把创建对象过程交给Spring实现。
   2. AOP：面向切面，在不修改源码的情况下进行功能增强。

4. Spring特点：

   1. 方便解耦，简化开发
   2. AOP编程支持
   3. 方便程序测试
   4. 方便和其他框架整合
   5. 方便进行实物操作
   6. 降低JavaApi的使用程度

5. 下载链接

   [repo.spring.io](https://repo.spring.io/ui/native/release/org/springframework/spring)

6. 版本5.2.18

## 0.2、入门案例

```Java
//1、编写 User 类
public class User {
    public void add(){
        System.out.println("add................");
    }
}
//2、引入Bean\Core\Context\Expression 包
//3、编写Spring配置文件 xml格式
<bean id="user" class="com.company.spring5.User"></bean>
// 4、编写测试类
import org.junit.Test;
public class TestSpring5 {
    @Test
    public void testAdd(){
        //加载Spring配置文件
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("bean1.xml");
        //获取配置创建对象
        User user = applicationContext.getBean("user",User.class);
        System.out.println(user);
        user.add();
    }
}
// 输出：
//		com.company.spring5.User@4923ab24
//		add................
```

# 1、IOC（控制反转）

## 1.1、IOC基础原理

xml解析、工厂模式、反射

## 1.2、IOC接口

IOC实现类有两个接口：

1. BeanFactory

   主要是Spring内部人员使用，不提供开发人员使用

2. ApplicationContext

   是BeanFactory的一个子接口，具有更多的功能

## 1.3、IOC操作Bean管理

### 1、什么是Bean管理

Bean管理主要是指两个操作：

1. Spring创造对象
2. Spring注入属性

### 2、Bean管理操作的两种实现方式

#### 1、基于xml配置文件方式实现

##### 1、基于xml实现类的创建

```Java
<bean id="user" class="com.company.spring5.User"></bean>
```

1. 在Spring配置文件中，使用bean标签，标签里添加对应属性，就可以实现对象创建。
2. bean标签的主要属性：
   1. id  类的唯一标识，用于识别该对象，不可以加特殊符号。
   2. class  类的全路径（从src到类文件）。
   3. name  用于标识类，跟id类似，但是很少用，可以加特殊符号。
3. 创建类对象时，默认调用无参构造方法完成对象创建。

##### 2、基于xml方式注入属性

###### 1、DI：依赖注入，注入属性。（在创建对象的基础上执行）

1. set方法进行注入

   1. 创建类，构造set方法

      ```Java
      public class Book {
          //属性
          private String bname;
          private String bauthor;
          //属性对应的set方法
          public void setBname(String bname) {
              this.bname = bname;
          }
          public void setBauthor(String bauthor) {
              this.bauthor = bauthor;
          }
      }
      ```

      

   2. 在Spring配置文件中配置对象创建，配置属性注入

      ```XML
      <!--使用set方法注入属性-->
      <bean id="book" class="com.company.spring5.Book">
          <!--使用property完成属性注入
                  name：   属性名
                  value：  属性值
              -->
          <property name="bname" value="金刚经"></property>
          <property name="bauthor" value="释迦摩尼"></property>
      </bean>
      ```

      

2. 使用带参构造方法进行注入

   1. 创建类，定义属性，创建属性对应的带参构造方法

      ```Java
      public class Order {
          //属性
          private String oname;
          private String address;
          //构造方法
          public Order(String oname, String address) {
              this.oname = oname;
              this.address = address;
          }
      }
      ```

   2. 在Spring配置文件中进行配置

      ```XML
      <!--使用带参构造方法完成属性注入-->
      <bean id="order" class="com.company.spring5.Order">
          <constructor-arg name="address" value="河南"></constructor-arg>
          <constructor-arg name="oname" value="第一条订单"></constructor-arg>
      </bean>
      ```

3. p名称空间注入

   1. 使用p名称空间注入，可以简化xml配置 

      ```XML
      <!--使用p名称空间注入-->
      <bean id="book" class="com.company.spring5.Book" p:bname="辟邪剑谱" p:bauthor="佚名"></bean>
      ```

###### 2、XML注入其他类型属性

1. 字面量

   1. null值

   ```XML
   <!--注入空值-->
   <bean id="book" class="com.company.spring5.Book">
       <property name="bname" value="金刚经"></property>
       <property name="bauthor">
           <null></null>
       </property>
   </bean>
   ```

2. 属性值包含特殊符号

   1. 把属性写入CDATA

      ```XML
      <!-- <<如来>> 为属性值-->
      <bean id="book" class="com.company.spring5.Book">
          <property name="bname" value="金刚经"></property>
          <property name="bauthor">
              <value><![CDATA[<<如来>>]]></value>
          </property>
      </bean>
      ```

   2. 字符转义

      ```XML
      <!--
      	字符表示
      	&lt	<
      	&gt	>
      	&le	<=
      	&ge	>=
      -->
      ```

3. 注入属性-- 外部bean

   ```Java
   public class UserServiceImpl implements UserService {
       //属性
       private UserDao userDao;
   	//set方法
       public void setUserDao(UserDao userDao) {
           this.userDao = userDao;
       }
       @Override
       public void print() {
           userDao.print();
       }
   }
   ```

   

   ```XML
   <bean id="userDao" class="com.company.spring5.dao.Impl.UserDaoImpl"></bean>
   <bean id="userService" class="com.company.spring5.service.Impl.UserServiceImpl">
       <!--注入userDao对象
                   name属性：类里面的属性名称
                   ref属性：创建userDao对象bean标签的id值
           -->
       <property name="userDao" ref="userDao"></property>
   </bean>
   ```

4. 注入属性--内部bean

   1. 一对多关系：部门和员工

   2. 在实体类之间表示一对多关系，员工表示所属部门，使用对象属性类型进行表示

      ```Java
      //部门类
      public class Dept {
          private String dname;
      
          public void setDname(String dname) {
              this.dname = dname;
          }
      }
      //员工类
      public class Emp {
          private String ename;
          private String gender;
          //员工属于某一部门，使用对象形式进行表示
          private Dept dept;
          public void setDept(Dept dept) {
              this.dept = dept;
          }
          public void setEname(String ename) {
              this.ename = ename;
          }
          public void setGender(String gender) {
              this.gender = gender;
          }
      }
      ```

   3. 在Spring配置文件中进行配置

      ```XML
      <!--内部bean-->
      <bean id="emp" class="com.company.spring5.bean.Emp">
          <!--先设置两个普通属性-->
          <property name="ename" value="王洋"></property>
          <property name="gender" value="男"></property>
          <!--设置对象属性-->
          <property name="dept">
              <bean id="dept" class="com.company.spring5.bean.Dept">
                  <property name="dname" value="总裁办"></property>
              </bean>
          </property>
      </bean>
      ```

5. 注入属性--级联赋值

   1. 第一种方法（外部bean）

      ```XML
      <bean id="emp" class="com.company.spring5.bean.Emp">
          <property name="ename" value="奥特曼"></property>
          <property name="gender" value="男"></property>
          <!--级联赋值-->
          <property name="dept" ref="dept"></property>
      </bean>
      <bean id="dept" class="com.company.spring5.bean.Dept">
          <property name="dname" value="武装部"></property>
      </bean>
      ```

   2. 第二种方法

      需要先在emp类中生成dept的get方法

      ```XML
      <bean ide="emp" class="com.company.spring5.bean.Emp">
          <property name="ename" value="奥特曼"></property>
          <property name="gender" value="男"></property>
          <!--级联赋值-->
          <property name="dept" ref="dept"></property>
          <property name="dept.dname" value="财务部"></property>
      </bean>
      <bean id="dept" class="com.company.spring5.bean.Dept">
          <property name="dname" value="武装部"></property>
      </bean>
      ```

6. 注入集合属性

   1. 注入数组类型属性

   2. 注入List类型属性

   3. 注入Map集合类型属性

   4. 注入Set集合类型属性

      ```Java
      //创建类，定义数组、List、Map、Set类型属性，生成对应的set方法
      public class Stu {
          //数组类型属性
          private String[] courses;
          //List集合类型属性
          private List<String> list;
          //Map集合类型的属性
          private Map<String,String> map;
          //set集合类型的属性
          private Set<String> sets;
          public void setCourses(String[] courses) {
              this.courses = courses;
          }
          public void setList(List<String> list) {
              this.list = list;
          }
          public void setMap(Map<String, String> map) {
              this.map = map;
          }
          public void setSets(Set<String> sets) {
              this.sets = sets;
          }
      }
      ```

      ```XML
      <!--集合类型属性注入-->
      <bean id="stu" class="com.company.spring5.collectiontype.Stu">
          <!--数组类型属性注入-->
          <property name="courses">
              <array>
                  <value>计算机组成原理</value>
                  <value>计算机网络原理</value>
                  <value>编译原理</value>
              </array>
          </property>
          <!--List类型属性注入-->
          <property name="list">
              <list>
                  <value>XYY</value>
                  <value>DYY</value>
                  <value>ZYY</value>
              </list>
          </property>
          <!--Map类型属性注入-->
          <property name="map">
              <map>
                  <entry key="Java" value="java"></entry>
                  <entry key="PHP" value="php"></entry>
              </map>
          </property>
          <!--Set集合类型属性注入-->
          <property name="sets">
              <set>
                  <value>MySQL</value>
                  <value>Redis</value>
              </set>
          </property>
      </bean>
      ```

   5. 对象类型集合属性注入

      ```XML
      <!--对象类型List集合注入-->
      <property name="courseList">
          <list>
              <ref bean="course1"></ref>
              <ref bean="course2"></ref>
          </list>
      </property>
      
      <bean id="course1" class="com.company.spring5.collectiontype.Course">
          <property name="cname" value="计组"></property>
      </bean>
      <bean id="course2" class="com.company.spring5.collectiontype.Course">
          <property name="cname" value="计网"></property>
      </bean>
      ```

   6. 提取集合类型属性注入

      1. Spring配置文件中引入名称空间util

         ```XML
         <?xml version="1.0" encoding="UTF-8"?>
         <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xmlns:util="http://www.springframework.org/schema/util"
                xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                    http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
         ```

      2. 使用 util:list 标签完成集合类型提取

         ```Java
         //创建类
         public class Book {
             private List<String> list;
             private Set<String> set;
             public void setList(List<String> list) {
                 this.list = list;
             }
             public void setSet(Set<String> set) {
                 this.set = set;
             }
             @Override
             public String toString() {
                 return "Book{" +
                     "list=" + list +
                     ", set=" + set +
                     '}';
             }
         }
         ```

         ```XML
         <!--Spring配置文件配置-->
         <util:list id="booklist">
             <value>天下第一</value>
             <value>血饮狂刀</value>
         </util:list>
         <util:set id="booklist2">
             <value>高能物理</value>
             <value>量子力学</value>
         </util:set>
         <bean id="book" class="com.company.spring5.collectiontype.Book">
             <property name="list" ref="booklist"></property>
             <property name="set" ref="booklist2"></property>
         </bean>
         ```

##### 3、Spring的两种bean类型

###### 1、普通bean

​		配置文件中定义的bean的类型就是返回的类型

###### 2、工厂bean

​		在配置文件中定义的bean类型可能不是返回类型

1. 创建一个类，让这个类作为工厂bean，实现接口FactoryBean

2. 实现接口的方法，在实现的方法中定义返回的bean类型

   ```Java
   public class MyBean implements FactoryBean<Course>{
       //定义返回bean
       @Override
       public Course getObject() throws Exception {
           Course course = new Course();
           course.setCname("高数");
           return course;
       }
       @Override
       public Class<?> getObjectType() {
           return null;
       }
       @Override
       public boolean isSingleton() {
           return FactoryBean.super.isSingleton();
       }
   }
   //测试类
   @org.junit.Test
       public void testBean3(){
       ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml");
       Course course = context.getBean("myBean", Course.class);
       System.out.println(course);
   }
   ```

   配置文件：

   ```XML
   <bean id="myBean" class="com.company.spring5.factorybean.MyBean"></bean>
   ```

##### 4、bean的作用域

1. 在Spring里设置创建bean实例是单实例还是多实例

2. 在Spring里默认情况下bean是单实例

   ![image-20211202201613722](D:\notes\笔记\后端\Spring.assets\image-20211202201613722.png)

3. 设置单实例和多实例

   1. 在Spring配置文件bean标签里有属性（scope）用于设置单实例还是多实例。

   2. scope属性值

      - 默认值 singleton，表示是单实例对象。

      - prototype，表示是多实例对象。

      ​	

      ```XML
      <bean id="myBean" scope="prototype" class="com.company.spring5.factorybean.MyBean"></bean>
      ```

      ![image-20211202201958351](D:\notes\笔记\后端\Spring.assets\image-20211202201958351.png)

   3. singleton和prototype的区别

      - singleton是单实例，prototype是多实例。

      - 设置scope值为singleton的时候，加载spring配置文件时就会创建单实例对象。

        设置scope值为prototype的时候，不是在加载Spring配置文件的时候创建对象，而是在调用getBean方法的时候创建多实例的对象。

##### 5、bean的生命周期

1. 生命周期

   从对象创建到销毁的过程。

2. bean生命周期

   1. 通过构造器创建bean实例（无参数构造）。
   2. 为bean的属性设置值和对其他bean调用（调用set方法）。
   3. 调用bean的初始化的方法（需要进行配置）。
   4. bean对象可以使用了（获取到了对象）。
   5. 当容器关闭时，调用bean的销毁方法（需要进行配置销毁的方法）。

3. 示例

   ```Java
   //创建类
   public class Orders {
       private String oname;
       public void setOname(String oname) {
           this.oname = oname;
           System.out.println("第二步、调用set方法，设定属性值");
       }
       //无参构造方法
       public Orders() {
           System.out.println("第一步、执行无参构造方法");
       }
       //创建初始化方法
       public void init(){
           System.out.println("第三步、执行初始化方法");
       }
       //创建销毁方法
       public void destory(){
           System.out.println("第五步、执行销毁方法");
       }
   }
   //测试类
   @org.junit.Test
       public void testBean4(){
       //ApplicationContext context = new ClassPathXmlApplicationContext("bean4.xml");
       ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean4.xml");
       Orders orders = context.getBean("orders",Orders.class);
       System.out.println("第四步、获取对象");
       System.out.println(orders);
       context.close();
   }
   ```

   配置文件：

   ```XML
   <bean id="orders" class="com.company.spring5.bean.Orders" init-method="init" destroy-method="destory">
       <property name="oname" value="手机"></property>
   </bean>
   ```

   输出：

   ```
   第一步、执行无参构造方法
   第二步、调用set方法，设定属性值
   第三步、执行初始化方法
   第四步、获取对象
   com.company.spring5.bean.Orders@51c8530f
   第五步、执行销毁方法
   ```

4. bean的后置处理器，bean生命周期有七步

   1. 通过构造器创建bean实例（无参数构造）。
   2. 为备案的属性设置值和对其他bean调用（调用set方法）。
   3. **把bean实例传递给bean后置处理器的方法。postProcessBeforeInitialization方法**
   4. 调用bean的初始化的方法（需要进行配置）。
   5. **把bean实例传递给bean后置处理器的方法。postProcessAfterInitialization方法**
   6. bean对象可以使用了（获取到了对象）。
   7. 当容器关闭时，调用bean的销毁方法（需要进行配置销毁的方法）。

5. 后置处理器演示

   1. 创建类，实现BeanPostProcessor接口，创建后置处理器

      ```Java
      public class BeanPost implements BeanPostProcessor {
          @Override
          public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
              System.out.println("初始化前执行的方法");
              return BeanPostProcessor.super.postProcessBeforeInitialization(bean, beanName);
          }
      
          @Override
          public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
              System.out.println("初始化之后执行的方法");
              return BeanPostProcessor.super.postProcessAfterInitialization(bean, beanName);
          }
      }
      ```

      配置文件：

      ```XML
      <!--配置后置处理器
       会为当前配置文件内的所有bean对象执行后置处理器
      -->
      <bean id="beanPost" class="com.company.spring5.bean.BeanPost"></bean>
      ```

      输出：

      ```
      第一步、执行无参构造方法
      第二步、调用set方法，设定属性值
      初始化前执行的方法
      第三步、执行初始化方法
      初始化之后执行的方法
      第四步、获取对象
      com.company.spring5.bean.Orders@c81cdd1
      第五步、执行销毁方法
      ```

      

##### 6、xml自动装配

1. 什么是自动装配

   1. 根据指定装配规则（属性名称或者属性类型），Spring自动将匹配的属性值进行注入

2. 演示：

   1. 通过属性名称实现自动注入

      ```XML
      <bean id="emp" class="com.company.spring5.autowire.Emp" autowire="byName"></bean>
      <bean id="dept" class="com.company.spring5.autowire.Dept"></bean>
      ```

      

   2. 通过属性类型实现自动注入

      ```XML
      <bean id="emp" class="com.company.spring5.autowire.Emp" autowire="byType"></bean>
      <bean id="dept" class="com.company.spring5.autowire.Dept"></bean>
      ```

      

##### 7、引入外部属性文件

1. 直接配置数据库信息

   1. 配置druid连接池

   2. 导入依赖jar包

      ```XML
      <!--直接配置连接池-->
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
          <property name="url" value="jdbc:mysql:localhost:3306/userDb"></property>
          <property name="username" value="root"></property>
          <property name="password" value="root"></property>
      </bean>
      ```

2. 引入外部属性文件

   1. 创建外部属性文件，properties属性文件

      ```
      prop.driverClass=com.mysql.jdbc.driver
      prop.url=jdbc:mysql://localhost:3306/userDb
      prop.username=root
      prop.password=root
      ```

      

   2. 把外部properties属性文件引入到Spring配置文件中

      ```XML
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:util="http://www.springframework.org/schema/util"
             xmlns:context="http://www.springframework.org/schema/context"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                 http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
                                 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
      
          <!--引入外部属性文件-->
          <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
          <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
              <property name="driverClassName" value="${prop.driverClass}"></property>
              <property name="url" value="${prop.url}"></property>
              <property name="username" value="${prop.username}"></property>
              <property name="password" value="${prop.password}"></property>
          </bean>
      </beans>
      ```

      



#### 2、基于注解方式实现

##### 1、什么是注解

- 注解是代码中的特殊标记，格式：@注解名称(属性名称1=属性值,属性名称2=属性值...)
- 使用注解，注解作用在类上面，方法上面，属性上面
- 使用注解的目的：简化XML配置

##### 2、Spring针对Bean管理中创建对象提供注解

- @Component
- @Service
- @Controller
- @Respository
- 上面四个注解功能是一样的，都可以用来创建bean实例

##### 3、基于注解方式实现对象创建

1. 引入依赖	AOP jar包

2. 开启组件扫描

   ```XML
   <!--开启组件扫描
           如果臊面多个包，可以使用逗号隔开
                       可以写到上一层的目录
       -->
   <context:component-scan base-package="com.company.spring5"></context:component-scan>
   ```

3. 创建类，在类上面添加创建对象注解

   ```Java
   //注解里value属性值可以省略不写
   //默认值是类名称首字母小写
   //UserService----userService
   @Component(value = "userService")//<bean id="userService" class="">
   public class UserService {
       public void test(){
           System.out.println("test UserService....");
       }
   }
   //测试类
   @org.junit.Test
   public void test1(){
       ApplicationContext context = new ClassPathXmlApplicationContext("bean6.xml");
       UserService userService = context.getBean("userService",UserService.class);
       userService.test();
   }
   ```

##### 4、开启组件扫描的细节配置

```XML
<!--
        use-default-filters="false" 表示现在不是用默认filter，自己配置filter
        context:include-filter 设置扫描那些内容
-->
<context:component-scan base-package="com.company.spring5" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
<!--
        context:exclude-filter  设置哪些内容不进行扫描
    -->
<context:component-scan base-package="com.company.spring5">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

##### 5、基于注解方式实现属性注入

1. @AutoWired：根据属性类型进行自动注入

   ```Java
   1、把service和dao对象创建，在service和dao类添加创建对象注解
   2、在service注入dao对象，在service类添加dao类型属性，在属性上面使用注解
   @Service
   public class UserService {
       /**
        * 定义Dao类型属性
        * 不需要添加set方法
        * 添加注入属性注解
        */
       @Autowired
       private UserDao userDao;
   
       public void test(){
           System.out.println("test UserService....");
           userDao.add();
       }
   }
   ```

   

2. @Qualifier：根据属性的名称进行注入

   ```Java
   要和@Autowired注解一起使用
       @Service
   public class UserService {
       /**
        * 定义Dao类型属性
        * 不需要添加set方法
        * 添加注入属性注解
        */
       @Autowired
       @Qualifier(value = "userDAoImpl1")//根据名称进行注入
       private UserDao userDao;
   
       public void test(){
           System.out.println("test UserService....");
           userDao.add();
       }
   }
   ```

   

3. @Resource：可以根据类型注入也可以根据名称注入（由javax包提供）

   ```Java
   //@Resource	//根据类型进行注解
   @Resource(name="userDAoImpl1")//根据名称进行注解
   private UserDao userDao;
   ```

   

4. @Value：注入普通类型属性

   ```Java
   @Value(value = "abc")
   private String name;
   ```

##### 6、纯注解开发

1. 创建配置类

   ```Java
   @Configuration  //作为配置类，替代xml配置文件
   @ComponentScan(basePackages = "com.company.spring5")//开启扫描
   public class SpringConfig {
   }
   ```

2. 编写测试类

   ```Java
   @Test
   public void test2() {
       //加载配置类
       ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
       UserService userService = context.getBean("userService", UserService.class);
       userService.test();
   }
   ```

# 2、AOP

## 2.1、基本概念

1. 什么是AOP
   1. 面向切面编程，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，提高了开发的效率。
   2. 不通过修改源代码方式，在主干功能里面添加新功能。

## 2.2、AOP底层原理

### 2.2.1、AOP底层使用动态代理

有两种情况动态代理

1. 有接口情况，使用JDK动态代理。

   创建接口实现类代理对象，增强类的方法。

   ![image-20220109151357770](D:\notes\笔记\后端\Spring.assets\image-20220109151357770.png)

2. 没有接口情况，使用CGLIB动态代理。

   创建子类的代理对象，增强类的方法。

   ![image-20220109151925863](D:\notes\笔记\后端\Spring.assets\image-20220109151925863.png)

### 2.2.2、JDK动态代理

JDK动态代理，使用Proxy类里面的方法创建代理对象。

1. 调用 newProxyInstance 方法

   ```Java
   newProxyInstance(ClassLoader loader, 类<?>[] interfaces, InvocationHandler h)
   返回指定接口的代理类的实例，该接口将方法调用分派给指定的调用处理程序。
   ```

   方法里有三个参数：

   - 类加载器
   - 增强方法所在类实现的接口，支持多个接口
   - 实现InvocationHandler接口，创建代理对象，写增强方法

2. 实现

   1. 创建接口，定义方法

      ```Java
      public interface UserDao {
          public int add(int a,int b);
          public String update(String id);
      }
      
      ```
      
   2. 创建接口实现类，实现方法

      ```Java
      public class UserDaoImpl implements UserDao {
          @Override
          public int add(int a, int b) {
              return a+b;
          }
      
          @Override
          public String update(String id) {
              return id;
          }
      }
      ```
   
   3. 使用Proxy类创建接口代理对象

      ```Java
      public class JdkProxy {
      
          public static void main(String[] args) {
              //    创建接口实现类的代理对象
              Class[] interfaces = {UserDao.class};
              //Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new InvocationHandler() {
              //    //  匿名内部类
              //    @Override
              //    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
              //        return null;
              //    }
              //});
              UserDaoImpl userDao = new UserDaoImpl();
              System.out.println(userDao);
              UserDao dao = (UserDao) Proxy.newProxyInstance(JdkProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));
              int add = dao.add(1, 2);
              System.out.println(add);
              String id = dao.update("HelloWorld!");
              System.out.println(id);
          }
      }
      
      /**
       * 创建代理对象代码
       */
      class UserDaoProxy implements InvocationHandler {
          /**
           * 创建哪个类的代理对象，就把哪个类传过来
           */
          private Object obj;
      
          public UserDaoProxy(Object obj) {
              this.obj = obj;
          }
          /**
           * 增强的逻辑
           *
           * @param proxy
           * @param method 增强的方法名
           * @param args   参数
           * @return
           * @throws Throwable
           */
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
      
              //方法之前
              System.out.println("方法前执行。" + method.getName() + "；传递的参数:" + Arrays.toString(args));
              //被增强的方法执行
              Object res = method.invoke(obj, args);
              // 方法后执行
              System.out.println("方法后执行" + obj);
              return res;
          }
      }
      ```


### 2.2.3、术语

1. 连接点

   类中可以增强的方法，称为连接点。

2. 切入点

   实际被真正增强的方法，称为切入点。

3. 通知（增强）

   实际增强的逻辑部分，称为通知（增强）。

   通知的几种类型：

   - 前置通知	@Before
   - 后置通知    @After
   - 环绕通知    @Around
   - 异常通知    @AfterThrowing
   - 最终通知    @AfterReturning

4. 切面 

   把通知应用到切入点的过程。

### 2.2.4、AOP操作（准备）

1. Spring框架一般都是基于AspectJ实现AOP操作

2. 什么是AspectJ？

   ```
   AspectJ不是Spring的组成部分，是独立的AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作。
   ```

3. 基于AspectJ实现AOP操作

   - 基于xml配置文件实现
   - 基于注解方式实现 （实际使用）

4. 在项目工程里引入相关依赖

   ![image-20220124172616226](D:\notes\笔记\后端\Spring.assets\image-20220124172616226.png)

5. 切入点表达式

   1. 切入点表达式作用：知道对哪个类里面的哪个方法进行增强。

   2. 语法结构

      ```
      execution([权限修饰符][返回类型][类全路径][方法名称]([参数列表]))
      例1：对 com.atguigu.dao.BookDao 类里面的 add 进行增强
      execution(* com.atguigu.dao.BookDao.add(..))
      例2：对 com.atguigu.dao.BookDao 类里面的所有方法进行增强
      execution(* com.atguigu.dao.BookDao.*(..))
      例3 ：对 com.atguigu.dao包里面的所有类， 类里面的所有方法进行增强
      execution(* com.atguigu.dao.*.*(..))
      ```


### 2.2.5、AspectJ注解

#### 1、步骤

1. 创建类，在类里定义方法

   ```Java
   public class User {
       public void add(){
           System.out.println("add......");
       }
   }
   ```

   

2.  创建增强类（编写增强逻辑）

   在增强的类里面，创建方法，让不同方法代表不同的通知类型

   ```Java
   //增强的类
   public class UserProxy {
       //前置通知
       public void before(){
           System.out.println("before......");
       }
   }
   ```

   

3. 进行通知的配置

   1. 在Spring配置文件中，开启注解扫描

      ```XML
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:context="http://www.springframework.org/schema/context"
             xmlns:aop="http://www.springframework.org/schema/aop"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                                 http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
          <!-- 开启注解扫描 -->
          <context:component-scan base-package="com.company.spring5.aopanno"></context:component-scan>
      </beans>
      ```

      

   2.  使用注解创建User 和UserProxy对象

      ```Java
      @Component
      public class User {
          public void add(){
              System.out.println("add......");
          }
      }
      ```

      ```Java
      @Component
      public class UserProxy {
          //前置通知
          public void before(){
              System.out.println("before......");
          }
      }
      ```

      

   3. 在增强类上面添加注解@Aspect

      ```Java
      @Component
      @Aspect
      public class UserProxy {
          //前置通知
          public void before(){
              System.out.println("before......");
          }
      }
      ```

      

   4. 在Spring配置文件中开启生成代理对象

      ```XML
      <!-- 开启Aspect代理对象 -->
      <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
      ```

      

4. 配置不同类型的通知

   在增强的类里面，在作为通知的方法上面添加通知类型注解，使用切入点表达式配置

   ```Java
   @Component
   @Aspect
   public class UserProxy {
       /**
        * @Before注解表示作为前置通知
        */
       @Before(value = "execution(* com.company.spring5.aopanno.User.add(..))")
       public void before() {
           System.out.println("before......");
       }
   
       /**
        * @After注解表示作为后置通知
        */
       @After(value = "execution(* com.company.spring5.aopanno.User.add(..))")
       public void after() {
           System.out.println("after......");
       }
   
       /**
        * @Around注解表示环绕通知
        */
       @Around(value = "execution(* com.company.spring5.aopanno.User.add(..))")
       public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
           System.out.println("环绕之前......");
           proceedingJoinPoint.proceed();
           System.out.println("环绕之后......");
       }
   
       /**
        * @AfterReturning注解表示作为最终通知
        */
       @AfterReturning(value = "execution(* com.company.spring5.aopanno.User.add(..))")
       public void afterReturning() {
           System.out.println("afterReturning......");
       }
   
       /**
        * @AfterThrowing注解表示作为异常通知
        */
       @AfterThrowing(value = "execution(* com.company.spring5.aopanno.User.add(..))")
       public void afterThrowing() {
           System.out.println("afterThrowing......");
       }
   }
   ```

5. 测试类

   ```Java
   public class AopTest {
       @Test
       public void testAopAnno(){
           ApplicationContext context = new ClassPathXmlApplicationContext("bean7.xml");
           User user = context.getBean("user",User.class);
           user.add();
       }
   }
   ```

#### 2、相同切入点抽取

```Java
@Component
@Aspect
public class UserProxy {
    /**
     * 相同切入点抽取
     */
    @Pointcut(value = "execution(* com.company.spring5.aopanno.User.add(..))")
    public void pointCut() {

    }

    /**
     * @Before注解表示作为前置通知
     */
    @Before(value = "pointCut()")
    public void before() {
        System.out.println("before......");
    }

    /**
     * @After注解表示作为后置通知
     */
    @After(value = "pointCut()")
    public void after() {
        System.out.println("after......");
    }

    /**
     * @Around注解表示环绕通知
     */
    @Around(value = "pointCut()")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前......");
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后......");
    }

    /**
     * @AfterReturning注解表示作为最终通知
     */
    @AfterReturning(value = "pointCut()")
    public void afterReturning() {
        System.out.println("afterReturning......");
    }

    /**
     * @AfterThrowing注解表示作为异常通知
     */
    @AfterThrowing(value = "pointCut()")
    public void afterThrowing() {
        System.out.println("afterThrowing......");
    }
}
```

#### 3、多个增强类，设置增强类优先级

在有多个增强类增强同一个方法的情况下，可以使用@Order(数字类型的值)注解设置增强类优先级，数字越小优先级越高

```Java
@Component
@Aspect
@Order(1)
public class PersonProxy {
    @Before(value = "execution(* com.company.spring5.aopanno.User.add(..))")
    public void before() {
        System.out.println("personBefore......");
    }
}
```

### 2.2.6、AspectJ配置文件

1. 创建两个类，被增强类和增强类，定义方法

   ```Java
   public class User {
       public void add(){
           System.out.println("add......");
       }
   }
   ```

   ```Java
   public class UserProxy {
       @Before(value = "execution(* com.company.spring5.aopanno.User.add(..))")
       public void before() {
           System.out.println("personBefore......");
       }
   }
   ```

2. 在配置文件中生成这两个类对象

   ```XML
   <bean id="user" class="com.company.spring5.aopxml.User"></bean>
   <bean id="userProxy" class="com.company.spring5.aopxml.UserProxy"></bean>
   ```

3. 在Spring配置文件中配置切入点

   ```XML
   <!--AOP配置-->
   <aop:config>
       <!--配置切入点-->
       <aop:pointcut id="pointCut" expression="execution(* com.company.spring5.aopxml.Book.add(..))"/>
       <!--配置切面-->
       <aop:aspect ref="bookProxy">
           <!--增强在具体的方法上-->
           <aop:before method="before" pointcut-ref="pointCut"/>
       </aop:aspect>
   </aop:config>
   ```

### 2.2.7、完全注解开发

创建配置类，不需要XML配置文件

```Java
@Configuration
@ComponentScan(basePackages = {"com.company.spring5"})
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class ConfigAop {
}
```

# 3、JdbcTemplate

### 3.1、什么是JdbcTemplate？

Spring框架对JDBC进行封装，使用JdbcTemplate方便实现对数据库进行操作

### 3.2、准备工作

1. 引入相关jar包 

   ![image-20220124215515971](D:\notes\笔记\后端\Spring.assets\image-20220124215515971.png)

2. 在Spring配置文件中配置数据库连接池

   ```XML
   <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="clone">
       <property name="url" value="jdbc:mysql:///user_db"></property>
       <property name="username" value="root"></property>
       <property name="password" value="root"></property>
       <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
   </bean>
   ```

3. 配置JdbcTemplate对象，注入DataSource

   ```XML
   <!--JdbcTemplate对象-->
   <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
       <!--注入dataSource-->
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   ```

4. 创建service类、dao类，在dap注入JdbcTemplate对象

   配置文件：

   ```XML
   <!--组件扫描-->
   <context:component-scan base-package="com.company.spring5"></context:component-scan>
   ```

   service注入dao：

   ```Java
   @Service
   public class BookService {
       /**
        * 注入dao
        */
       @Autowired
       private BookDao bookDao;
   }
   ```

   dao注入JdbcTemplate：

   ```Java
   @Repository
   public class BookDaoImpl implements BookDao{
       /**
        * 注入JdbcTemplate
        */
       @Autowired
       private JdbcTemplate jdbcTemplate;
   
   }
   ```

### 3.3、操作数据库

对应数据库中的表，创建实体类

```Java
public class Book {
    private String bookId;
    private String bookName;
    private String bookStatus;
    public String getBookId() {
        return bookId;
    }
    public void setBookId(String bookId) {
        this.bookId = bookId;
    }
    public String getBookName() {
        return bookName;
    }
    public void setBookName(String bookName) {
        this.bookName = bookName;
    }
    public String getBookStatus() {
        return bookStatus;
    }
    public void setBookStatus(String bookStatus) {
        this.bookStatus = bookStatus;
    }
}
```

#### 3.3.1、添加操作(insert)

编写service和dao

1. 在dao进行数据库添加操作

   ```Java
   /**
   * 添加方法
   * @param book
   */
   @Override
   public void add(Book book) {
       String sql = "insert into t_book values(?,?,?)";
       int update = jdbcTemplate.update(sql, book.getBookId(), book.getBookName(), book.getBookStatus());
       System.out.println(update);
   }
   ```

2. 调用JdbcTemplate对象里面的update方法实现添加操作

   update(String sql,Object... args)

   - sql	要执行的sql语句
   - Object... args  可变长参数，要设置的sql语句值

   ```Java
   /**
   * 添加方法
   * @param book
   */
   @Override
   public void add(Book book) {
       String sql = "insert into t_book values(?,?,?)";
       int update = jdbcTemplate.update(sql, book.getBookId(), book.getBookName(), book.getBookStatus());
       System.out.println(update);
   }
   ```

3. 测试类

   ```Java
   public class BookServiceTest {
   
   @Test
       public void addBook() {
       Object object;
       ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
       BookService bookService = context.getBean("bookService",BookService.class);
       Book book = new Book();
       book.setBookId("1");
       book.setBookName("《万历十五年》");
       book.setBookStatus("onSale");
       bookService.addBook(book);
       }
   }
   ```

#### 3.3.2、修改操作(update)

dao：

```Java
public void updateBook(Book book);
```

daoImpl:

```Java
/**
* 修改方法
* @param book
*/
@Override
public void updateBook(Book book) {
	String sql = "update t_Book set bbookname=?,bookstatus=? where book_id=?" ;
    int update = jdbcTemplate.update(sql,book.getBookName(),book.getBookStatus(),book.getBookId());
    System.out.println(update);
}
```

service:

```Java
/**
* 修改的方法
* @param book
*/
public void updateBook(Book book){
    bookDao.updateBook(book);
}
```

#### 3.3.3、删除操作(delete)

dao:

```Java
public void deleteById(String id);
```

daoImpl:

```Java
/**
* 删除方法
* @param id    要删除条目的id
*/
@Override
public void deleteById(String id) {
    String sql = "delete from t_book where book_id=?" ;
    int update = jdbcTemplate.update(sql, id);
    System.out.println(update);
}
```

service:

```Java
public void deleteBook(String id){
    bookDao.deleteById(id);
}
```

#### 3.3.4、查询操作(select)

执行 queryForObject 方法实现查询操作

queryForObject (String sql,Class<T> requiredType)

- String sql							要执行的sql语句
- Class<T> requiredType     返回类型class

service:

```Java
public int count(){
    return bookDao.selectCount();
}
```

dao：

```Java
int selectCount();
```

daoImpl：

```Java
@Override
public int selectCount() {
    String sql = "select count(*) from t_book";
    Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
    return count;
}
```

#### 3.3.5、查询返回对象

调用 queryForObject(String sql,RowMapper<T> rowMapper,Object... args) 方法实现查询返回对象

三个参数：

- String sql								 执行的sql语句
- RowMapper<T> rowMapper   rowMapper是一个接口，针对返回不同类型数据，用来实现数据封装
- Object... args						  sql语句参数

```Java
@Override
public Book selectBookById(String id) {
    String sql = "select * from t_book where book_id=?";
    Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
    return book;
}
```

#### 3.3.6、查询返回集合

调用 query(String sql,RowMapper<T> rowMapper,Object... args) 方法实现查询返回集合

三个参数：

- String sql								 执行的sql语句
- RowMapper<T> rowMapper   rowMapper是一个接口，针对返回不同类型数据，用来实现数据封装
- Object... args						  sql语句参数

```Java
@Override
public List<Book> selectAllBook() {
    String sql ="select * from t_book";
    List<Book> bookList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
    return bookList;
}
```

#### 3.3.7、批量添加

调用 batchUpdate(String sql,List<Object[]> batchArgs) 方法实现批量添加操作

两个参数：

- String sql							要执行的sql语句
- List<Object[]> batchArgs   list集合，添加多条记录

```Java
@Override
public void batchAddBook(List<Object[]> batchArgs) {
    String sql = "insert into t_book values(?,?,?)";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
```

测试类：

```Java
@Test
public void batchAdd() {
    List<Object[]> batchArgs = new ArrayList<>();
    Object[] o1 = {"4","放风筝的人","onSale"};
    Object[] o2 = {"5","明朝败家子","onSale"};
    Object[] o3 = {"6","百词斩","onSale"};
    batchArgs.add(o1);
    batchArgs.add(o2);
    batchArgs.add(o3);
    bookService.batchAdd(batchArgs);
}
```

#### 3.3.8、批量修改

要注意list集合中数据的位置要和sql语句中对应

```Java
@Override
public void batchUpdateBook(List<Object[]> batchArgs) {
    String sql = "update t_Book set bookname=?,bookstatus=? where book_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
```

测试类：

```Java
@Test
public void batchUpdate() {
    List<Object[]> batchArgs = new ArrayList<>();
    Object[] o1 = {"放风筝的人","Sale","4"};
    Object[] o2 = {"明朝败家子","Sale","5"};
    Object[] o3 = {"百词斩","Sale","6"};
    batchArgs.add(o1);
    batchArgs.add(o2);
    batchArgs.add(o3);
    bookService.batchUpdate(batchArgs);
}
```

#### 3.3.9、批量删除

```Java
@Override
public void batchDeleteBook(List<Object[]> batchArgs) {
    String sql = "delete from t_book where book_id=?";
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
    System.out.println(Arrays.toString(ints));
}
```

测试类：

```Java
@Test
public void batchDelete() {
    List<Object[]> batchArgs = new ArrayList<>();
    Object[] o1 = {"4"};
    Object[] o2 = {"5"};
    Object[] o3 = {"6"};
    batchArgs.add(o1);
    batchArgs.add(o2);
    batchArgs.add(o3);
    bookService.batchDelete(batchArgs);
}
```

# 4、事务操作

## 4.1、什么是事务？

1. 事务是数据库操作最基本单元，是逻辑上的一组操作，要么都成功，如果有一个失败则所有的操作都失败

2. 典型场景：银行转账

   A转账100元给B

   A减少100元，B增加100元

## 4.2、事务的四个特性(ACID)

1. 原子性

   原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚，因此事务的操作如果成功就必须要完全应用到数据库，如果操作失败则不能对数据库有任何影响。

2. 一致性

   一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。举例来说，假设用户A和用户B两者的钱加起来一共是1000，那么不管A和B之间如何转账、转几次账，事务结束后两个用户的钱相加起来应该还得是1000，这就是事务的一致性。

3. 隔离性

   隔离性是当多个用户并发访问数据库时，比如同时操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。

4. 持久性

   持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。例如我们在使用JDBC操作数据库时，在提交事务方法后，提示用户事务操作完成，当我们程序执行完成直到看到提示后，就可以认定事务已经正确提交，即使这时候数据库出现了问题，也必须要将我们的事务完全执行完成。否则的话就会造成我们虽然看到提示事务处理完毕，但是数据库因为故障而没有执行事务的重大错误。这是不允许的。

## 4.3、搭建实务操作环境

1. 创建数据库表，添加记录

   ![image-20220127201444568](D:\notes\笔记\后端\Spring.assets\image-20220127201444568.png)

2. 创建service，搭建dao，完成对象创建和注入关系。

   service注入dao，dao注入jdbcTemplate，JdbcTemplate注入dataSource

   ```Java
   @Service
   public class UserService {
       @Autowired
       private UserDao userDao;
   }
   ```

   ```Java
   public interface UserDao {
   }
   ```

   ```Java
   @Repository
   public class UserDaoImpl implements UserDao{
       @Autowired
       private JdbcTemplate jdbcTemplate;
   }
   ```

3. 事务操作流程

   ```Java
   public void account(){
       try {
       //    第一步，开启事务
   
       //    第二步，进行业务操作
   
       //    第三步，没有异常提交事务
   
       }catch (Exception e){
       //    第四步，出现异常，事务回滚
       }
   }
   ```

## 4.4、Spring事务管理介绍

1. 事务添加到JavaEE三层结构里面的Service层(业务逻辑层)

2. 在Spring中进行事务管理操作，有两种方式：

   1. 编程式事务管理(上述实务操作流程就是编程式事务管理，不常用)
   2. 声明式事务管理(使用)

3. 声明式事务管理

   1. 基于注解方式
   2. 基于xml配置文件方式

4. 在Spring进行声明式事务管理，底层使用AOP原理

5. Spring事务管理API

   ```
   PlatformTransactionManager 是事务管理器接口，针对不同的框架提供不同的实现类
   ```

   ![image-20220127210821458](D:\notes\笔记\后端\Spring.assets\image-20220127210821458.png)

## 4.5、注解 声明式事务管理

1. 在Spring配置文件中配置事务管理器

   ```XML
   <!--创建事务管理器-->
   <bean id="transaction" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <!--注入数据源-->
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   ```

2. 在Spring配置文件中，开启事务注解

   1. 引入名称空间 tx

      ```XML
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:context="http://www.springframework.org/schema/context"
             xmlns:aop="http://www.springframework.org/schema/aop"      
             xmlns:tx="http://www.springframework.org/schema/tx"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                                 http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
                                 http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
      ```

   2. 开启事务注解

      ```XML
      <tx:annotation-driven transaction-manager="transaction"></tx:annotation-driven>
      ```

3. 在service类上面(或者service类里面的方法上面)添加事务注解

   1. @Transactional注解可以添加在类上面也可以添加在方法上面
      - 添加在类上面，表示类里面的所有方法都添加事务
      - 添加在方法上面，只表示该方法添加事务

   ```Java
   @Service
   @Transactional(rollbackFor = )
   public class UserService {}
   ```

## 4.6、@Transactional注解参数

1. ![image-20220127215032166](D:\notes\笔记\后端\Spring.assets\image-20220127215032166.png)

2. propagation：事务传播行为

   - 多个事务方法直接进行调用，这个过程是如何管理的

   - 事务方法：对数据库进行变化的操作（增、删、改）

   - Spring 七种传播行为：

     ![20170420212829825](D:\notes\笔记\后端\Spring.assets\20170420212829825.png)

   ```Java
   @Service
   @Transactional(propagation = Propagation.REQUIRED)
   public class UserService {}
   ```

   

3. isolation：事务隔离级别

   1. 事务的隔离性：多事务操作之间不会产生影响。

   2. 不考虑隔离性会产生三个读问题，需要通过设置隔离级别来解决问题：

      - 脏读

        ```
        指一个线程中的事务读取到了另外一个线程中未提交的数据。
        
        脏读又称无效数据的读出，是指在数据库访问中，事务T1将某一值修改，然后事务T2读取该值，此后T1因为某种原因撤销对该值的修改，这就导致了T2所读取到的数据是无效的，值得注意的是，脏读一般是针对于update操作的。
        ```

      - 不可重复读

        ```
        指一个线程中的事务读取到了另外一个线程中提交的update的数据。
        
        在一个事务内，多次读同一个数据。在这个事务还没有结束时，另一个事务也访问该同一数据并修改数据。那么，在第一个事务的两次读数据之间。由于另一个事务的修改，那么第一个事务两次读到的数据可能不一样，这样就发生了在一个事务内两次读到的数据是不一样的，因此称为不可重复读，即原始读取不可重复。
        ```

      - 虚(幻)读

        ```
        指一个线程中的事务读取到了另外一个线程中提交的insert的数据。
        
        事务T1读取满足某种搜索条件的一些行，然后事务T2插入了符合T1的搜索条件的一个新行。如果T1重新执行产生原来那些行的查询，就会得到不同的行。
        ```

   3. 事务的隔离级别

      | 隔离级别                   | 脏读   | 不可重复读 | 虚读(幻读) |
      | :------------------------- | ------ | ---------- | ---------- |
      | 读未提交(READ UNCOMMITTED) | 可能   | 可能       | 可能       |
      | 读提交(READ COMMITTED)     | 不可能 | 可能       | 可能       |
      | 可重复读(REPEATABLE READ)  | 不可能 | 不可能     | 可能       |
      | 串行化(SERIALIZABLE)       | 不可能 | 不可能     | 不可能     |

   ```Java
   @Service
   @Transactional(propagation = Propagation.REQUIRED,isolation = Isolation.SERIALIZABLE)
   public class UserService {}
   ```

4. timeout：超时时间

   1. 事务需要在一定时间内进行提交，如果不提交进行回滚
   2. 默认值是-1，设置时间疫苗单位进行计算

5. readOnly：是否只读

   1. 读：查询操作；写：添加修改删除操作
   2. readOnly默认值false，表示可以查询，可以添加修改删除操作
   3. 设置readOnly值是true，设置成true之后，只能查询

6. rollbackFor：回滚

   - 设置出现哪些异常进行事务回滚

7. noRollbackFor：不回滚

   - 设置出现哪些异常不进行事务回滚

## 4.7、XML 声明式事务管理

1. 在Spring配置文件中进行配置
   1. 第一步 配置事务管理器
   
      ```XML
      <!--开启组件扫描-->
      <context:component-scan base-package="com.company.spring5"></context:component-scan>
      <!--创建数据源dataSource-->
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <!--注入属性-->
          <property name="url" value="jdbc:mysql:///user_db"></property>
          <property name="username" value="root"></property>
          <property name="password" value="root"></property>
          <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
      </bean>
      <!--jdbcTemplate对象-->
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
          <!--注入数据源-->
          <property name="dataSource" ref="dataSource"></property>
      </bean>
      <!--1.创建事务管理器-->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <!--注入数据源-->
          <property name="dataSource" ref="dataSource"></property>
      </bean>
      ```
   
   2. 第二步 配置通知
   
      ```XML
      <!--2.创建通知-->
      <tx:advice id="txAdvice">
          <!--配置通知相关属性-->
          <tx:attributes>
              <!--指定哪种规则的方法上面添加事务-->
              <tx:method name="account" propagation="REQUIRED"/>
              <!--<tx:method name="account*"></tx:method>-->
          </tx:attributes>
      </tx:advice>
      ```
   
   3. 第三步 配置切入点和切面
   
      ```XML
      <!--3.配置切入点和切面-->
      <aop:config>
          <!--配置切入点-->
          <aop:pointcut id="pointCut" expression="execution(* com.company.spring5.service.UserService.*(..))"/>
          <!--配置切面-->
          <aop:advisor advice-ref="txAdvice" pointcut-ref="pointCut"/>
      </aop:config>
      ```

## 4.8、完全注解开发

创建配置类，使用配置类代替XML配置文件

```Java
@Configuration  //配置类
@ComponentScan(basePackages = "com.company.spring5")    //开启组件扫描
@EnableTransactionManagement    //开启事务管理
public class TxConfig {

    /**
     * 创建数据库连接池
     * @return
     */
    @Bean
    public DruidDataSource getDruidDataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl("jdbc:mysql:///user_db");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        return dataSource;
    }

    /**
     * 创建jdbcTemplate对象
     * @param dataSource
     * @return
     */
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource){
        //在IOC容器中根据类型找到dataSource
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //注入dataSource
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    /**
     * 创建事务管理器
     * @param dataSource
     * @return
     */
    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }
}
```

# 5、Spring5新特性

## 5.1、Spring框架核心容器支持    @Nullable注解

@Nullable注解可以用在方法、属性、参数上面，表示方法返回值可以为空，属性值可以为空，参数值可以为空。

- 注解使用在方法上面	表示方法返回值可以为空

  ```Java
  @Nullable
  String getId();
  ```

- 注解使用在方法参数上面    表示方法的参数值可以为空

  ```Java
  public <T> void registerBean(@Nullable String beanName, Class<T> beanClass, @Nullable Supplier<T> supplier, BeanDefinitionCustomizer... customizers) {
  this.reader.registerBean(beanClass, beanName, supplier, customizers);
  }
  ```

- 注解使用在属性上面   表示属性值可以为空

  ```Java
  @Nullable
  private Resource[] configResources;
  ```

## 5.2、Spring框架核心容器支持  函数式风格GenericApplicationContext/AnnotationConfigApplicationContext

函数式风格创建对象，交给Spring管理

```Java
public void testGenericApplicationContext() {
    //创建GenericApplicationContext对象
    GenericApplicationContext genericApplicationContext = new GenericApplicationContext();
    //genericApplicationContext的方法注册对象
    genericApplicationContext.refresh();
    //genericApplicationContext.registerBean(User.class,() -> new User());
    //User user= (User) genericApplicationContext.getBean("com.company.spring5.entity.User");
    genericApplicationContext.registerBean("user1",User.class,() -> new User());
    //获取在Spring注册的对象
    User user = (User) genericApplicationContext.getBean("user1");
    System.out.println(user);
}
```

## 5.3、Spring5 支持整合JUint5

### 5.3.1、整合JUint4

1. 引入Spring相关针对测试的依赖

![image-20220208215519265](D:\notes\笔记\后端\Spring.assets\image-20220208215519265.png)

![image-20220208221647075](D:\notes\笔记\后端\Spring.assets\image-20220208221647075-16443298141091.png)

2. 创建测试类，使用注解方式完成基本过程

   ```Java
   @RunWith(SpringJUnit4ClassRunner.class)     //单元测试框架
   @ContextConfiguration("classpath:bean2.xml")    //加载配置文件
   public class JUint4Test {
       @Autowired
       private UserService userService;
       @Test
       public void test(){
           userService.account();
       }
   }
   ```

### 5.3.2、整合JUint5

1. 引入JUnit5依赖

2. 创建测试类，使用注解方式完成基本过程

   ```Java
   @ExtendWith(SpringExtension.class)
   @ContextConfiguration("classpath:bean2.xml")
   public class JUnit5Test {
       @Autowired
       private UserService userService;
       @Test
       public void test(){
           userService.account();
       }
   }
   ```

3. 复合注解

   ```Java
   @SpringJUnitConfig(locations = "classpath:bean2.xml")相当于
   @ExtendWith(SpringExtension.class)
   @ContextConfiguration("classpath:bean2.xml")这两个注解
   ```

## 5.4、WebFlux

### 5.4.0、SpringWebflux介绍

1. SpringWebflux是Spring5新添加的模块，用于web开发，功能和SpringMVC类似，webflux是一种当前比较流行的响应式编程出现的框架。
2. 使用基于Servlet容器的传统web框架，例如SpringMVC。Webflux是一种异步非阻塞的框架，异步非阻塞框架在Servlet3.1以后才支持，核心是基于Reactor的相关API实现的。
3. 什么是异步非阻塞？
   - 异步和同步：针对调用者而言。如果调用者得等待请求返回才继续工作就是同步；如果调用者发出请求就去做其他事情就是异步。
   - 阻塞和非阻塞：针对被调用者而言。阻塞时，在调用结果返回前，当前线程会被挂起，并在得到结果之后返回。非阻塞时，如果不能立刻得到结果，则该调用者不会阻塞当前线程。
4. Webflux特点:
   1. 非阻塞性：在有限资源下，提高系统吞吐量和伸缩性，以Reactor为基础实现响应式编程。
   2. 函数式编程：可以使用Java8中函数式编程方式实现路由请求。

### 5.4.1、相关知识

- SpringMVC
- SpringBoot
- Maven
- Java8新特性

### 5.4.2、响应式编程 

1. 是什么是响应式编程
   
   > 响应式编程是一种面向数据流和变化传播的编程范式。这意味着可以在编程语言中很方便地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。
   > 电子表格程序就是响应式编程的一个例子。单元格可以包含字面值或类似"=B1+C1"的公式，而包含公式的单元格的值会依据其他单元格的值的变化而变化。
   
2. Java8及其之前版本
   
   > 提供 观察者模式 相关的两个类 Observer和Observable
   
3. 

### 5.4.3、Webflux执行流程和核心API

### 5.4.4、SpringWebflux(基于注解编程模型)

### 5.4.5、SpringWebflux(基于函数式编程模型)



