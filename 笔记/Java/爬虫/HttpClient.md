# 1.HttpClient

**介绍：**HttpClient是apache旗下的一个开源项目

**作用：**实现页面的抓取

# 2、工程搭建

1.  创建maven项目
2.  添加依赖到pom.xml文件

```XML
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>
```

# 3、使用

## 3.1 发送Gct请求使用方法

**步骤：**

1.  创建一个Httpclient对象（CloseableHttpClient），HttpClients创建。（相当于打开浏览器）
2.  创建一个HttpGet对象，相当于是一个get请求，构造参数中设置请求的url。
3.  使用HttpC1ient对象发送请求。
4.  得到服务谣的响应。
5.  打印html
6.  关闭流

**测试类**

```JAVA
public class HttpClientTest {
    @Test
    public void testGet() throws Exception{
        // 1.  创建一个Httpclient对象（CloseableHttpClient），HttpClients创建。（相当于打开浏览器）
        CloseableHttpClient httpClient = HttpClients.createDefault();
        // 2.  创建一个HttpGet对象，相当于是一个get请求，构造参数中设置请求的url.
        HttpGet get = new HttpGet("https://www.xrmn5.cc/plus/search/index.asp?keyword=%E6%9E%97%E6%98%9F%E9%98%91&searchtype=title");
        // 3.  使用HttpC1ient对象发送请求，
        CloseableHttpResponse response = httpClient.execute(get);
        // 4.  得到服务谣的响应。取服务端相应的html
        //获得状态行
        StatusLine statusLine = response.getStatusLine();
        System.out.println(statusLine);
        //获取状态码
        int statusCode = statusLine.getStatusCode();
        System.out.println(statusCode);
        //获取服务端响应内容html
        //response.getEntity() 获取到的是inputstream流，需要用EntityUtils工具将它转换成字符串
        HttpEntity entity = response.getEntity();
        String html = EntityUtils.toString(entity, "UTF-8");
        // 5.  打印html
        System.out.println(html);
        // 6.  关闭流
        response.close();
        httpClient.close();
    }
}
```



## 3.2 发送Post请求

**步骤：**

1.  创建一个Httpclient对象
2.  创建一个HttpPost
3.  创建一个HttpEntity对象，相当于是表单
4.  把表单添动加到post对象中
5.  使用HttpClient发送post请求
6.  接收服务端响应的html
7.  打印html
8.  关闭流

**测试类**

```JAVA
 @Test
    public void testPost() throws Exception{
        // 1.  创建一个Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        // 2.  创建一个HttpPost
        HttpPost post = new HttpPost("http://localhost/emp/login");
        //创建一个list，用来存储参数
        List<NameValuePair> form = new ArrayList<>();
        //添加参数
        // form.add(new BasicNameValuePair("参数名","参数"));
        // 3.  创建一个HttpEntity对象，相当于是表单
        HttpEntity httpEntity = new UrlEncodedFormEntity(form);
        // 4.  把表单添动加到post对象中
        post.setEntity(httpEntity);
        post.setEntity(httpEntity);
        // 5.  使用HttpClient发送post请求
        CloseableHttpResponse response = httpClient.execute(post);
        // 6.  接收服务端响应的html
        String html = EntityUtils.toString(response.getEntity(), "UTF-8");
        // 7.  打印html
        System.out.println(html);
        // 8.  关闭流
        response.close();
        httpClient.close();
    }
```

# 4、HttpClient连接池

**使用：**

1.  创建一个连接池对象，PoolingHttpClientConnectionManager
2.  创建HttpClient时，还是使用HttpClients工具，使用customer()，然后设置连接池
3.  创建一个HttpGet对象
4.  发送请求接收结果
5.  打印结果
6.  关闭response对象（HttpClient对象不需要关闭）