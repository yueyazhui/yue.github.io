## RuoYi-Vue 集成 UReport2

### 集成

#### 后端

在 ruoyi-common 模块的 pom.xml 中添加依赖

```java
 <!-- UReport2 报表 -->
<dependency>
    <groupId>com.bstek.ureport</groupId>
    <artifactId>ureport2-console</artifactId>
    <version>${ureport2.version}</version>
</dependency>
```

在项目的 pom.xml 中添加 UReport2 的版本属性

```java
<ureport2.version>2.2.9</ureport2.version>
```

在项目的启动类中添加注解并初始化Bean

```java
/**
 * 启动程序
 * 
 * @author ruoyi
 */
@ImportResource("classpath:ureport-console-context.xml")
@SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })
public class RuoYiApplication
{
    public static void main(String[] args)
    {
        // System.setProperty("spring.devtools.restart.enabled", "false");
        SpringApplication.run(RuoYiApplication.class, args);
        System.out.println("(♥◠‿◠)ﾉﾞ  若依启动成功   ლ(´ڡ`ლ)ﾞ  \n" +
                " .-------.       ____     __        \n" +
                " |  _ _   \\      \\   \\   /  /    \n" +
                " | ( ' )  |       \\  _. /  '       \n" +
                " |(_ o _) /        _( )_ .'         \n" +
                " | (_,_).' __  ___(_ o _)'          \n" +
                " |  |\\ \\  |  ||   |(_,_)'         \n" +
                " |  | \\ `'   /|   `-'  /           \n" +
                " |  |  \\    /  \\      /           \n" +
                " ''-'   `'-'    `-..-'              ");
    }

    @Bean
    public ServletRegistrationBean ureportServlet(){
        return new ServletRegistrationBean(new UReportServlet(), "/ureport/*");
    }
}
```


在 ruoyi-framework 模块的 SecurityConfig 中将 /urepoer/** 设置为允许匿名访问

`ruoyi-framework\src\main\java\com\ruoyi\framework\config\SecurityConfig.java`

```java
httpSecurity
        // CSRF禁用，因为不使用session
        .csrf().disable()
        // 认证失败处理类
        .exceptionHandling().authenticationEntryPoint(unauthorizedHandler).and()
        // 基于token，所以不需要session
        .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS).and()
        // 过滤请求
        .authorizeRequests()
        // 对于登录login 注册register 验证码captchaImage 允许匿名访问
        .antMatchers("/login", "/register", "/captchaImage").anonymous()
        // 静态资源，可匿名访问
        .antMatchers(HttpMethod.GET, "/", "/*.html", "/**/*.html", "/**/*.css", "/**/*.js", "/profile/**", "/favicon.ico", "/static/**").permitAll()
        .antMatchers("/swagger-ui.html", "/swagger-resources/**", "/webjars/**", "/*/api-docs", "/druid/**").permitAll()
        // ureport 允许匿名访问
        .antMatchers("/ureport/**").anonymous()
        // 除上面外的所有请求全部需要鉴权认证
        .anyRequest().authenticated()
        .and()
        .headers().frameOptions().disable();
```

启动项目

UReport2 报表的访问地址：http://localhost:11111/ureport/designer（小编的端口为：11111）

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307221208951.png)

#### 前端

在 ruoyi-ui/vue.config.js 中添加代理对象

```javascript
'/ureport': {
  target: 'http://localhost:11111',
  ws: false,
  changeOrigin: true,
  pathRewrite: {
    '^/ureport': '/ureport'
  }
}
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307221138289.png)


在 views 目录下添加 ureport/designer/index.vue 文件

`ruoyi-ui/src/views/ureport/designer/index.vue`

```vue
<template>
  <div v-loading="loading" :style="'height:'+ height">
    <iframe :src="src" style="width: 100%;height: 100%;border: none;overflow: auto"/>
  </div>
</template>

<script>
export default {
  name: "UReport",
  data() {
    return {
      src: "/ureport/designer",
      height: document.documentElement.clientHeight - 94.5 + "px;",
      loading: true
    }
  },
  mounted: function() {
    setTimeout(() => {
      this.loading = false
    }, 200)
    const that = this
    window.onresize = function temp() {
      that.height = document.documentElement.clientHeight - 94.5 + "px;"
    }
  }
}
</script>
```

运行若依管理系统，依次点击系统管理/菜单管理，新增目录

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307221159956.png)

在当前目录下新增菜单

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307221203002.png)

重新登录

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307221205010.png)

### 配置

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222135345.png)

config.properties

```properties
# 服务器文件系统对应的报表文件地址
ureport.fileStoreDir=/Users/yue/own/temp/ureport/file
# 是否禁用服务器文件系统
ureport.disableFileProvider=false
# 配置 UReport 根路径，对应 ureport-console/src/main/resources/ureport-console-context.xml 中的 ureport.designerServletAction
ureport.contextPath=/
```

context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <import resource="classpath:ureport-console-context.xml"/>
    <bean id="propertyConfigurer" parent="ureport.props">
        <property name="locations">
            <list>
                <value>classpath:ureport/config.properties</value>
            </list>
        </property>
    </bean>
</beans>
```

修改启动类上的注解

```java
@ImportResource("classpath:ureport/context.xml")
@SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })
public class RuoYiApplication
{
    public static void main(String[] args)
    {
        // System.setProperty("spring.devtools.restart.enabled", "false");
        SpringApplication.run(RuoYiApplication.class, args);
        System.out.println("(♥◠‿◠)ﾉﾞ  若依启动成功   ლ(´ڡ`ლ)ﾞ  \n" +
                " .-------.       ____     __        \n" +
                " |  _ _   \\      \\   \\   /  /    \n" +
                " | ( ' )  |       \\  _. /  '       \n" +
                " |(_ o _) /        _( )_ .'         \n" +
                " | (_,_).' __  ___(_ o _)'          \n" +
                " |  |\\ \\  |  ||   |(_,_)'         \n" +
                " |  | \\ `'   /|   `-'  /           \n" +
                " |  |  \\    /  \\      /           \n" +
                " ''-'   `'-'    `-..-'              ");
    }

    @Bean
    public ServletRegistrationBean ureportServlet(){
        return new ServletRegistrationBean(new UReportServlet(), "/ureport/*");
    }
}
```

启动项目，添加数据源（三种方式）

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222141031.png)

1. 直连

   ![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222147163.png)

2. Spring Bean

   ![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222148086.png)

   ![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222150527.png)

3. 内置数据源

   ![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222151689.png)

### 测试

数据：

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222114110.png)

设计：

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222118517.png)

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222117306.png)

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222120477.png)

预览：

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/UReport2_202307222121931.png)

新增：http://localhost:11111/ureport/designer

编辑：http://localhost:11111/ureport/designer?_u=file:test.ureport.xml

查看：http://localhost:11111/ureport/preview?_u=file:test.ureport.xml



文档 https://www.w3cschool.cn/ureport/ureport-jaod2h8k.html

视频 https://b23.tv/xzRYk9K

