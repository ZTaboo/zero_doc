## 创建项目脚手架服务器

`https://start.aliyun.com` // 版本不全，目前已经3.0却只有2.7的版本

`https://start.springboot.io` // 国内速度不错的脚手架

## 控制器（controller）装配

添加`@RestController` 注解，它默认使用了`@Controller` 和`@ResponseBody` 注解，在spring boot 2.5版本中新增

## Hello World

```java
package com.example.demo1.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;


@RestController
public class Hello {
    public static class User {
        public String name;
        public int age;
    }
    @RequestMapping
    public String hello1() {
        return "hello world";
    }

    @RequestMapping(value = "/user", method = {RequestMethod.GET, RequestMethod.POST})
    public List<User> hello() {
        User u = new User();
        List<User> body = new ArrayList<>();

        u.name = "zero";
        u.age = 123;
        body.add(u);
        System.out.printf("name:%s,age:%d", u.name, u.age);
        return body;
    }
}
```

## application.properties配置记录

```.properties
server.port=1207
```

## 请求数据解析记录

### Get解析

```java
// RestFul风格参数解析
@RequestMapping(value = "/searchIdCard/{idCard}", method = RequestMethod.GET)
    private JSONObject searchIdCard(@PathVariable("idCard") String idCard) {
        return service.searchIdCard(idCard);
    }
  
// 传统query传递的参数
@RequestMapping(value = "/searchIdCard", method = RequestMethod.GET)
    private JSONObject searchIdCard1(@RequestParam("idCard") String idCard) {
        return service.searchIdCard(idCard);
    }
```

‍
