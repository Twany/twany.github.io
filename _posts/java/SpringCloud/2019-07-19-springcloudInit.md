---
layout: post
title: "SpringCloud初踩坑"
subtitle:   "SpringCloud + SpringBoot第一次的接触"
author:     "Twany"
header-img: "img/bg-scene/post-bg-ele.jpg"
catalog: true
tags:
    - SpringBoot
    - SpringCloud
---

## 什么是SpringCloud
**微服务架构的解决方案，是很多组件的集合**

- eureka: 注册中心，服务的注册和实现
- zuul: 网关组件，路由请求，过滤器（ribbon hystrix）
- ribbon: 负载均衡组件
- hystrix: 熔断组件
- feign: 远程调用组件（ribbon hystrix）
## Spring的RestTemplate
> Spring提供了一个RestTemplate模板工具类，对基于Http的客户端进行了封装，并且实现了对象与json的序列化和反序列化，非常方便。RestTemplate并没有限定Http的客户端类型，而是进行了抽象，目前常用的3种都有支持：
> - HttpClient
> - OkHttp
> - JDK原生的URLConnection（默认的）

## 项目启动
#### 启动类
- 首先在项目中注册一个`RestTemplate`对象，可以在启动类位置注册
    ```java
    @SpringBootApplication
    public class DemoApplication {

        @Bean
        public RestTemplate restTemplate(){
            return new RestTemplate();
        }

        public static void main(String[] args) {
            SpringApplication.run(DemoApplication.class, args);
        }

    }
    ```

- 然后是pojo类(必须要有getter,setter和toString方法)
    ```java
    package com.itcast.service.pojo;

    public class User{

        private Long id;

        private String username;

        private String phone;

        private String password;

        private String salt;

        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", username='" + username + '\'' +
                    ", phone='" + phone + '\'' +
                    ", password='" + password + '\'' +
                    ", salt='" + salt + '\'' +
                    ", created='" + created + '\'' +
                    '}';
        }

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getUsername() {
            return username;
        }

        public void setUsername(String username) {
            this.username = username;
        }

        public String getPhone() {
            return phone;
        }

        public void setPhone(String phone) {
            this.phone = phone;
        }

        public String getPassword() {
            return password;
        }

        public void setPassword(String password) {
            this.password = password;
        }

        public String getSalt() {
            return salt;
        }

        public void setSalt(String salt) {
            this.salt = salt;
        }

        public String getCreated() {
            return created;
        }

        public void setCreated(String created) {
            this.created = created;
        }

        private String created;

    }
    ```

- 控制器类
    ```java
    package com.itcast.service.controller;

    import com.itcast.service.pojo.User;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.*;
    import org.springframework.web.client.RestTemplate;

    @Controller
    @RequestMapping("consumer/user")
    public class UserController {

        @Autowired
        private RestTemplate restTemplate;

        @GetMapping
        @ResponseBody
        public User queryUserById(@RequestParam("id")int id){
            return this.restTemplate.getForObject("http://localhost:8081/user/" + id, User.class);
        }
    }

    ```
    - `@ResponseBody` 负责将返回数据写入 `HTTP response body` 中（json格式）
    - `@RequestParam` 为url 尾的 `?id=xxx` 形式
    - `@PathVariable` 为url 尾的 `/id/2` 形式

## 总结
##### SpringCloud个人感觉是利用不同的容器，每一个容器负责一个服务。提供对应的地址和端口，由其他服务来引用的概念。