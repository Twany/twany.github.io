---
layout: post
title: "SpringBoot采坑日记"
subtitle: "SpringBoot"
author: "Twany"
header-img: "img/bg-person/post-bg-re.jpg"
header-mask: 0.4
catelog: true
tags:
  - SpringBoot
  - Java
---

# SpringBoot采坑指南



> 没耐心学ssh，ssm，只是粗略的过了一遍。很早就听闻了springboot的强大，今日，终于迎来了实战



## prom\.xml配置

springboot自带dependency：

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

此处配置我们需要用到的依赖


* spring\-web（springmvc用到）

```xml

<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

* thymeleaf

```xml

<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```


* [webjars](https://www.webjars.org/)



## 静态文件目录结构


![静态文件目录结构](https://i.loli.net/2019/06/18/5d08af6aaea2a63487.png)




## thymeleaf语法的应用

#### thymeleaf提供 StringUtils 工具集，有点好用



## Controller使用

*  springboot提供 @PostMapping\(value = "\) 等方式代替 @Mapping


## springMVC

```java
//@EnableWebMvc 完全接管
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("login");
    }

    @Bean
    public WebMvcConfigurer webMvcConfigurer(){
        WebMvcConfigurer config = new WebMvcConfigurer() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/index.html").setViewName("login");
                registry.addViewController("/user/login").setViewName("login");
                registry.addViewController("/main.html").setViewName("dashboard");
            }

            //注册拦截器
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                //已经做好了静态界面拦截
                registry.addInterceptor(new LoginHandlerInterceptor())
                        .addPathPatterns("/**")
                        .excludePathPatterns("/index.html","/","/user/login","/asserts/**"); //还需要放行静态文件
            }
        };

        return config;
    }
}
```


## 拦截器

```java

//登录状态拦截器
public class LoginHandlerInterceptor implements HandlerInterceptor{
    //目标方法执行之前
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object user = request.getSession().getAttribute("loginUser");
        if (user == null){
            //未登录
            request.setAttribute("msg","没有权限请先登录");
            request.getRequestDispatcher("/").forward(request,response);
            return false;
        }else{
            //已登录
            return true;
        }

    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {

    }
}

```








































