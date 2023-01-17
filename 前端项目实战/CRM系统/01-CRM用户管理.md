
# CRM 用户管理

## 学习目标

![](../../markdown_img/Pasted%20image%2020230117010756.png)

## CRM 系统概念与项目开发流程

### CRM基本概念

CRM，即客户关系管理，客户关系管理是指企业为提高核心竞争力，利用相应的信息技术以及互联网技术协调企业与顾客间在销售、营销和服务上的交互，从而提升其管理方式，向客户提供创新式的个性化的客户交互和服务的过程。其最终目标是吸引新客户、保留老客户以及将已有客户转为忠实客户，增加市场。CRM是一种旨在健全、改善企业与客户之间关系的新型管理系统，CRM也是一种把客户信息转换成良好的客户关系的可重复性过程。

### CRM分类

**根据目标用户群体的不同**，CRM可以分为**2B-CRM、2C-CRM**两种类型。

**2B-CRM**具有以下特征：

-   目标用户群体：企业或者组织；
-   产品/服务：为企业提供解决经营管理类问题的产品或服务，客单价相对较高且复杂；
-   购买频率：低，通常以年为单位，短时间内不会重复购买；
-   涉及人员：多，企业内部相关人员均会多次参与；
-   决策周期：长，一般会经过初次接触阶段、产品试用阶段、产品决策等阶段；
-   面销：需要销售人员面对面去跟进进行销售 ；
-   关注点：企业的组织架构和人员角色，有利于提高签单效率；

**2C-CRM**具有以下特征：

-   目标用户群体：个人；
-   产品/服务：为解决个人需求的产品或服务，客单价相对较低且简单，但要求多样性；
-   购买频率：高，和个人生活结合的越紧密，其购买频率也就越高；
-   涉及人员：少，最多是全体家庭成员；
-   决策周期：短，很多人都会进行冲动消费；
-   面销：不需要销售，个人可自主购买，最多是电话销售 ；
-   关注点：个人信息和关系影响力，影响力越大，客户价值越大；

**根据系统功能划分**，可以将CRM划分为**OCRM、MCRM**两种。

OCRM

Operational CRM

操作型客户关系管理，即销售过程管理CRM，主要的系统功能是对线索的转换，赋能销售，帮其实现客户群体A到客户群体B的转换。为了能最大程度的转换，需要根据业务流程对线索的生命周期及转化流程进行管理，例如客户转换需要经历的阶段、客户的跟进记录、通话录音等。通常电销团队所使用的CRM即为此类型。

MCRM

Marketing CRM

营销型客户关系管理，主要的系统功能是营销管理，在客户全生命周期的管理中发挥作用。营销不仅是新客户的获取，还包括老客户留存和复购。在客户细分（客户分类、客户画像等）的基础上，进行精准营销，通过二次销售，实现客户的续费或复购。反映到图中，就是实现客户群体B从客户群体C、D、E的转换。通常电商类CRM即为此类型。

SCRM

Social CRM

社会化客户关系管理，主要是通过微信、QQ、微博、抖音等社交平台，对目标用户进行营销。例如现在的社区团购，通过企业微信对社群中的微信用户进行运营，从而实现销售的目的。

从图中可以看到，SCRM不仅可以用于发现潜在客户A，实现转换，也可以帮助企业更加了解已成交客户，实现从B到C、D、E的转换，所以SCRM其实是存在OCRM和MCRM中的。



### 企业项目开发流程

![](../../markdown_img/Pasted%20image%2020230117011556.png)

1. 产品组根据市场调研或商户同事的反馈提出idea,设计出原型然后跟市场,商户同事进行确认
2. UI设计组和开发组一起讨论，确定方案是否可行
3. UI组根据产品组提供的原型稿做出设计稿，与产品和开发确认
4. 开发组根据产品的原型稿(看逻辑)和UI组的设计稿(看界面)编写代码其中当然也会来回跟设计,产品同学进行确
认和沟通
5. 代码编写完毕后提交给测试组，然后再提交上线
6. 后期的数据跟踪和优化

这就是一个产品研发的大致流程。其中开发的责任就是选用合适的框架技术来完成产品所提供的需求以及设计所提供的效果。

## CRM系统模块划分

### 系统功能模块图

![](../../markdown_img/Pasted%20image%2020230117013410.png)

### 模块功能描述

**基础模块**

包含系统基本的用户登录，退出，记住我，密码修改等基本操作。

**营销管理**

营销机会管理：企业客户的质询需求所建立的信息录入功能，方便销售人员进行后续的客户需求跟踪。

营销开发计划：开发计划是根据营销机会而来，对于企业质询的客户，会有相应的销售人员对于该客户进行具体的沟通交流，此时对于整个Crm系统而言，通过营销开发计划来进行相应的信息管理，提高客户的购买企业产品的可能性。

**客户管理**

客户信息管理： Crm 系统中完整记录客户信息来源的数据、企业与客户交往、客户订单查询等信息录入功能,方便企业与客户进行相应的信息交流与后续合作。

客户流失管理： Crm 通过一-定规则机制所定义的流失客户(无效客户)，通过该规则可以有效管理客户信息资



## CRM系统数据库设计

CRM系统确定产品的原型稿以及UI组的设计稿，接下来就要设计数据库，-般在大公司通常会有专门的DBA,这时我们可以不要考虑数据库表设计，但是也要能够读懂或者了解DBA的设计思路,方便在程序开发阶段不会出现问题，一般关系型数据库表设计满足三范式的设计即可，表名设计做到见名知意最好。

## E-R图表简介

### 营销管理模块

![](../../markdown_img/Pasted%20image%2020230117153006.png)

### 客户管理模块

**客户信息管理**

![](../../markdown_img/Pasted%20image%2020230117153942.png)

**客户流失管理**

![](../../markdown_img/Pasted%20image%2020230117154134.png)

### 服务管理

![](../../markdown_img/Pasted%20image%2020230117154223.png)

### 系统管理

**权限模块**

![](../../markdown_img/Pasted%20image%2020230117154830.png)

**字典&日志管理**

![](../../markdown_img/Pasted%20image%2020230117154926.png)


## 项目搭建

### 创建项目

![](../../markdown_img/Pasted%20image%2020230117160602.png)

![](../../markdown_img/Pasted%20image%2020230117160648.png)


### 添加坐标&插件

在`pom.xml`文件中，添加项目集成所需的依赖坐标以及插件

```xml
<?xml version="1.0" encoding="UTF-8"?>  
  
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  
  <groupId>com.xxxx</groupId>  
  <artifactId>crm</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <!-- 设置打包类型为 war包 -->  
  <packaging>war</packaging>  
  
  <name>crm</name>  
  <!-- FIXME change it to the project's website -->  
  <url>http://www.example.com</url>  
  
  <properties>  
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
    <maven.compiler.source>1.8</maven.compiler.source>  
    <maven.compiler.target>1.8</maven.compiler.target>  
  </properties>  
  
  
  <parent>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <version>2.2.2.RELEASE</version>  
  </parent>  
  
  
  <dependencies>  
  
    <!-- 排除内部Tomcat的影响 -->  
    <dependency>  
      <groupId>org.springframework.boot</groupId>  
      <artifactId>spring-boot-starter-tomcat</artifactId>  
      <scope>provided</scope>  
    </dependency>  
  
    <!-- web 环境 -->  
    <dependency>  
      <groupId>org.springframework.boot</groupId>  
      <artifactId>spring-boot-starter-web</artifactId>  
    </dependency>  
    <!-- aop -->  
    <dependency>  
      <groupId>org.springframework.boot</groupId>  
      <artifactId>spring-boot-starter-aop</artifactId>  
    </dependency>  
    <!-- freemarker -->  
    <dependency>  
      <groupId>org.springframework.boot</groupId>  
      <artifactId>spring-boot-starter-freemarker</artifactId>  
    </dependency>  
    <!-- 测试环境 -->  
    <dependency>  
      <groupId>org.springframework.boot</groupId>  
      <artifactId>spring-boot-starter-test</artifactId>  
      <scope>test</scope>  
    </dependency>  
  
    <!-- mybatis -->  
    <dependency>  
      <groupId>org.mybatis.spring.boot</groupId>  
      <artifactId>mybatis-spring-boot-starter</artifactId>  
      <version>2.1.1</version>  
    </dependency>  
  
    <!-- 分页插件 -->  
    <dependency>  
      <groupId>com.github.pagehelper</groupId>  
      <artifactId>pagehelper-spring-boot-starter</artifactId>  
      <version>1.2.13</version>  
    </dependency>  
  
    <!-- 打包上线时需要忽略配置文件中的mysql -->  
    <!-- mysql -->    <dependency>  
      <groupId>mysql</groupId>  
      <artifactId>mysql-connector-java</artifactId>  
      <scope>runtime</scope>  
    </dependency>  
  
    <!-- c3p0 -->  
    <dependency>  
      <groupId>com.mchange</groupId>  
      <artifactId>c3p0</artifactId>  
      <version>0.9.5.5</version>  
    </dependency>  
  
    <!-- commons-lang3 -->  
    <dependency>  
      <groupId>org.apache.commons</groupId>  
      <artifactId>commons-lang3</artifactId>  
      <version>3.5</version>  
    </dependency>  
  
    <!-- json -->  
    <dependency>  
      <groupId>com.alibaba</groupId>  
      <artifactId>fastjson</artifactId>  
      <version>1.2.47</version>  
    </dependency>  
  
    <!-- DevTools 热部署 -->  
    <dependency>  
      <groupId>org.springframework.boot</groupId>  
      <artifactId>spring-boot-devtools</artifactId>  
      <optional>true</optional>  
    </dependency>  
  
  </dependencies>  
  
  <build>  
    <!-- 设置构建的war文件的名称 -->  
    <finalName>crm</finalName>  
    <plugins>  
      <plugin>  
        <groupId>org.apache.maven.plugins</groupId>  
        <artifactId>maven-compiler-plugin</artifactId>  
        <version>2.3.2</version>  
        <configuration>  
          <source>1.8</source>  
          <target>1.8</target>  
          <encoding>UTF-8</encoding>  
        </configuration>  
      </plugin>  
      <plugin>  
        <groupId>org.mybatis.generator</groupId>  
        <artifactId>mybatis-generator-maven-plugin</artifactId>  
        <version>1.3.2</version>  
        <configuration>  
          <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>  
          <verbose>true</verbose>  
          <overwrite>true</overwrite>  
        </configuration>  
      </plugin>  
      <plugin>  
        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-maven-plugin</artifactId>  
        <configuration>  
          <!-- 如果没有该配置，热部署的devtools不生效 -->  
          <fork>true</fork>  
        </configuration>  
      </plugin>  
    </plugins>  
  </build>  
    
</project>
```

### 添加配置文件

`src/main/resources/application.yml`

```yml
## 端口号  上下文路径  
server:  
  port: 8080  
  servlet:  
    context-path: /crm  
  
## 数据源配置  
spring:  
  datasource:  
    type: com.mchange.v2.c3p0.ComboPooledDataSource  
    driver-class-name: com.mysql.jdbc.Driver  
    url: jdbc:mysql://127.0.0.1:3306/crm?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8  
    username: root  
    password: 123456  
  
  ## freemarker  
  freemarker:  
    suffix: .ftl  
    content-type: text/html  
    charset: UTF-8  
    template-loader-path: classpath:/views/  
  
  ## 启用热部署  
  devtools:  
    restart:  
      enabled: true  
      additional-paths: src/main/java  
  
  
## mybatis 配置  
mybatis:  
  mapper-locations: classpath:/mappers/*.xml  
  type-aliases-package: com.xxxx.crm.vo;com.xxxx.crm.query;com.xxxx.crm.dto  
  configuration:  
    map-underscore-to-camel-case: true  
  
## pageHelper 分页  
pagehelper:  
  helper-dialect: mysql  
  
## 设置 dao 日志打印级别  
logging:  
  level:  
    com:  
      xxxx:  
        crm:  
          dao: debug
```


### 添加视图转发

新建 `com.xxxx.crm.controller`包，添加系统登陆，主页面转发

`controller`

```java
package com.xxxx.crm.controller;  
  
import com.xxxx.crm.base.BaseController;  
import com.xxxx.crm.service.PermissionService;  
import com.xxxx.crm.service.UserService;  
import com.xxxx.crm.utils.LoginUserUtil;  
import com.xxxx.crm.vo.User;  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.RequestMapping;  
  
import javax.annotation.Resource;  
import javax.servlet.http.HttpServletRequest;  
import java.util.List;  
  
@Controller  
public class IndexController extends BaseController {  
  
    @Resource  
    private UserService userService;  
  
    @Resource  
    private PermissionService permissionService;  
  
    /**  
     * 系统登录页  
     *  
     * @param  
     * @return java.lang.String  
     */    
    @RequestMapping("index")  
    public String index(){  
        return "index";  
    }  
  
    /**  
     * 系统界面欢迎页  
     *  
     * @param  
     * @return java.lang.String  
     */    
    @RequestMapping("welcome")  
    public String welcome(){  
        return "welcome";  
    }  
  
    /**  
     * 后端管理主页面  
     *  
     * @param  
     * @return java.lang.String  
     */    
    @RequestMapping("main")  
    public String main(HttpServletRequest request){  
  
        // 获取cookie中的用户Id  
        Integer userId = LoginUserUtil.releaseUserIdFromCookie(request);  
        // 查询用户对象，设置session作用域  
        User user = userService.selectByPrimaryKey(userId);  
        request.getSession().setAttribute("user",user);  
  
        // 通过当前登录用户ID查询当前登录用户拥有的资源列表 （查询对应资源的授权码）  
        List<String> permissions = permissionService.queryUserHasRoleHasPermissionByUserId(userId);  
        // 将集合设置到session作用域中  
        request.getSession().setAttribute("permissions", permissions);  
  
        return "main";  
    }  
  
}
```

### 添加静态资源

将 `public` 文件夹添加到 `src/main/resources/` 下

### 添加视图模板

`src/main/resources/`  下创建 `views` 目录，并天机文件

`index.ftl`

```html
<!DOCTYPE html>  
<html>  
<head>  
    <meta charset="UTF-8" />  
    <title>后台管理-登录</title>  
    <#include "common.ftl" />  
    <link rel="stylesheet" href="${ctx}/css/index.css" media="all" />  
</head>  
<body>  
<div class="layui-container">  
    <div class="admin-login-background">  
        <div class="layui-form login-form">  
            <form class="layui-form" action="">  
                <div class="layui-form-item logo-title">  
                    <h1>CRM后端登录</h1>  
                </div>  
                <div class="layui-form-item">  
                    <label class="layui-icon layui-icon-username" for="username"></label>  
                    <input type="text" name="username" lay-verify="required|account" placeholder="用户名或者邮箱" autocomplete="off" class="layui-input" />  
                </div>  
                <div class="layui-form-item">  
                    <label class="layui-icon layui-icon-password" for="password"></label>  
                    <input type="password" name="password" lay-verify="required|password" placeholder="密码" autocomplete="off" class="layui-input" />  
                </div>  
                <#-- 记住我 --/>                
                <div class="layui-form-item">  
                    <input type="checkbox" name="rememberMe" id="rememberMe" value="true" lay-skin="primary" title="记住密码" />  
                </div>  
                <div class="layui-form-item">  
                    <button class="layui-btn layui-btn-fluid" lay-submit="" lay-filter="login">登 录</button>  
                </div>  
            </form>  
        </div>  
    </div>  
</div>  
<script src="${ctx}/js/index.js" charset="utf-8"></script>  
</body>  
</html>
```

`main.ftl`

```html
<!DOCTYPE html>  
<html>  
<head>  
    <meta charset="utf-8"/>  
    <title>CRM-智能办公系统</title>  
    <#include "common.ftl"/>  
</head>  
<body class="layui-layout-body layuimini-all">  
<div class="layui-layout layui-layout-admin">  
    <div class="layui-header header">  
        <div class="layui-logo">  
            <a href="">  
                <img src="images/logo.png" alt="logo">  
                <h1>CRM-智能办公</h1>  
            </a>  
        </div>  
        <a>  
            <div class="layuimini-tool"><i title="展开" class="fa fa-outdent" data-side-fold="1"></i></div>  
        </a>  
        <ul class="layui-nav layui-layout-right">  
            <li class="layui-nav-item mobile layui-hide-xs" lay-unselect>  
                <a href="javascript:;" data-check-screen="full"><i class="fa fa-arrows-alt"></i></a>  
            </li>  
            <li class="layui-nav-item layuimini-setting">  
                <a href="javascript:;">${(user.userName)!""}</a>  
                <dl class="layui-nav-child">  
                    <dd>  
                        <a href="javascript:;" data-iframe-tab="${ctx}/user/toSettingPage" data-title="基本资料" data-icon="fa fa-gears">基本资料</a>  
                    </dd>  
                    <dd>  
                        <a href="javascript:;" data-iframe-tab="${ctx}/user/toPasswordPage" data-title="修改密码" data-icon="fa fa-gears">修改密码</a>  
                    </dd>  
                    <dd>  
                        <a href="javascript:;" class="login-out">退出登录</a>  
                    </dd>  
                </dl>  
            </li>  
            <li class="layui-nav-item layuimini-select-bgcolor mobile layui-hide-xs" lay-unselect>  
                <a href="javascript:;"></a>  
            </li>  
        </ul>  
    </div>  
  
    <div class="layui-side layui-bg-black">  
        <div class="layui-side-scroll layui-left-menu">  
            <#-- 判断当前登录用户是否拥有权限 -->            <#if permissions??>  
            <ul class="layui-nav layui-nav-tree layui-left-nav-tree layui-this" id="currency">  
                <#-- 通过freemarker中的seq_contains内建指令判断菜单是否显示 -->                <#if permissions?seq_contains("10") >  
                <li class="layui-nav-item">  
                    <a href="javascript:;" class="layui-menu-tips"><i class="fa fa-street-view"></i><span class="layui-left-nav"> 营销管理</span> <span class="layui-nav-more"></span></a>  
                    <dl class="layui-nav-child">  
                        <#if permissions?seq_contains("1010")>  
                        <dd>  
                            <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-1" data-tab="sale_chance/index" target="_self"><i class="fa fa-tty"></i><span class="layui-left-nav"> 营销机会管理</span></a>  
                        </dd>  
                        </#if>  
                        <#if permissions?seq_contains("1020")>  
                        <dd>  
                            <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-2" data-tab="cus_dev_plan/index" target="_self"><i class="fa fa-ellipsis-h"></i><span class="layui-left-nav"> 客户开发计划</span></a>  
                        </dd>  
                        </#if>  
                    </dl>  
                </li>  
                </#if>  
                <#if permissions?seq_contains("20") >  
                <li class="layui-nav-item">  
                    <a href="javascript:;" class="layui-menu-tips"><i class="fa fa-flag"></i><span class="layui-left-nav"> 客户管理</span> <span class="layui-nav-more"></span></a><dl class="layui-nav-child">  
                        <dd>  
                            <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-3" data-tab="customer/index" target="_self"><i class="fa fa-exchange"></i><span class="layui-left-nav"> 客户信息管理</span></a>  
                        </dd>  
                        <dd>  
                            <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-4" data-tab="customer_loss/index" target="_self"><i class="fa fa-user-times"></i><span class="layui-left-nav"> 客户流失管理</span></a>  
                        </dd>  
                    </dl>  
                </li>  
                </#if>  
                <#if permissions?seq_contains("30") >  
                <li class="layui-nav-item">  
                    <a href="javascript:;" class="layui-menu-tips"><i class="fa fa-desktop"></i><span class="layui-left-nav"> 服务管理</span> <span class="layui-nav-more"></span></a>  
                    <dl class="layui-nav-child">  
                            <dd>  
                                <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-5" data-tab="customer_serve/index/1" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 服务创建</span></a>  
                            </dd>  
                            <dd>  
                                <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-6" data-tab="customer_serve/index/2" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 服务分配</span></a>  
                            </dd>  
                            <dd>  
                                <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-7" data-tab="customer_serve/index/3" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 服务处理</span></a>  
                            </dd>  
                            <dd>  
                                <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-8" data-tab="customer_serve/index/4" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 服务反馈</span></a>  
                            </dd>  
                            <dd>  
                                <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-9" data-tab="customer_serve/index/5" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 服务归档</span></a>  
                            </dd>  
                    </dl>  
                </li>  
                </#if>  
                <#if permissions?seq_contains("40") >  
                <li class="layui-nav-item">  
                <a href="javascript:;" class="layui-menu-tips"><i class="fa fa-home"></i><span class="layui-left-nav"> 统计报表</span> <span class="layui-nav-more"></span></a><dl class="layui-nav-child">  
                    <dd>  
                        <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-10" data-tab="report/0" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 客户贡献分析</span></a>  
                    </dd>  
                    <dd>  
                        <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-10" data-tab="report/1" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 客户构成分析</span></a>  
                    </dd>  
                    <dd>  
                        <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-10" data-tab="report/2" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 客户服务分析</span></a>  
                    </dd>  
                    <dd>  
                        <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-10" data-tab="report/3" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 客户流失分析</span></a>  
                    </dd>  
                </dl>  
            </li>  
                </#if>  
                <#if permissions?seq_contains("60") >  
                <li class="layui-nav-item">  
                        <a href="javascript:;" class="layui-menu-tips"><i class="fa fa-gears"></i><span class="layui-left-nav"> 系统设置</span> <span class="layui-nav-more"></span></a>  
                        <dl class="layui-nav-child">  
                            <dd>  
                                <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-10" data-tab="data_dic/index" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 字典管理</span></a>  
                            </dd>  
                                <dd>  
                                    <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-11" data-tab="user/index" target="_self"><i class="fa fa-user"></i><span class="layui-left-nav"> 用户管理</span></a>  
                                </dd>  
                                <dd class="">  
                                    <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-12" data-tab="role/index" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 角色管理</span></a>  
                                </dd>  
                                <dd class="">  
                                    <a href="javascript:;" class="layui-menu-tips" data-type="tabAdd" data-tab-mpi="m-p-i-13" data-tab="module/index" target="_self"><i class="fa fa-tachometer"></i><span class="layui-left-nav"> 菜单管理</span></a>  
                                </dd>  
                        </dl>  
                    </li>  
                </#if>  
                <span class="layui-nav-bar" style="top: 201px; height: 0px; opacity: 0;"></span>  
            </ul>  
            </#if>  
        </div>  
    </div>  
  
    <div class="layui-body">  
        <div class="layui-tab" lay-filter="layuiminiTab" id="top_tabs_box">  
            <ul class="layui-tab-title" id="top_tabs">  
                <li class="layui-this" id="layuiminiHomeTabId" lay-id="welcome"><i class="fa fa-home"></i> <span>首页</span></li>  
            </ul>  
  
            <ul class="layui-nav closeBox">  
                <li class="layui-nav-item">  
                    <a href="javascript:;"> <i class="fa fa-dot-circle-o"></i> 页面操作</a>  
                    <dl class="layui-nav-child">  
                        <dd><a href="javascript:;" data-page-close="other"><i class="fa fa-window-close"></i> 关闭其他</a></dd>  
                        <dd><a href="javascript:;" data-page-close="all"><i class="fa fa-window-close-o"></i> 关闭全部</a></dd>  
                    </dl>  
                </li>  
            </ul>  
            <div class="layui-tab-content clildFrame">  
                <div id="layuiminiHomeTabIframe" class="layui-tab-item layui-show">  
                </div>  
            </div>  
        </div>  
    </div>  
  
</div>  
  
<script type="text/javascript" src="${ctx}/js/main.js"></script>  
</body>  
</html>
```

### 启动类

```java
package com.xxxx.crm;  
  
import org.mybatis.spring.annotation.MapperScan;  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.boot.builder.SpringApplicationBuilder;  
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;  
import org.springframework.scheduling.annotation.EnableScheduling;  
  
@SpringBootApplication  
@MapperScan("com.xxxx.crm.dao")  
@EnableScheduling // 启用定时任务  
public class Starter extends SpringBootServletInitializer {  
  
    public static void main(String[] args) {  
        SpringApplication.run(Starter.class);  
    }  
  
    /* 设置Web项目的启动入口 */    
    @Override  
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {  
        return builder.sources(Starter.class);  
    }  
}
```

## 自动生成代码

`resouces` 下，创建配置文件 `generatorConfig`

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE generatorConfiguration  
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"  
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">  
  
<generatorConfiguration>  
  
    <!--  
        数据库驱动  
            在左侧project边栏的External Libraries中找到mysql的驱动，右键选择copy path  
    -->    <classPathEntry  location="C:\Users\lenovo\.m2\repository\mysql\mysql-connector-java\5.1.47\mysql-connector-java-5.1.47.jar"/>  
  
    <context id="DB2Tables" targetRuntime="MyBatis3">  
  
        <commentGenerator>  
            <!-- 是否去除日期那行注释 -->  
            <property name="suppressDate" value="true"/>  
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->  
            <property name="suppressAllComments" value="true"/>  
        </commentGenerator>  
  
        <!-- 数据库链接地址账号密码 -->  
        <jdbcConnection  
                driverClass="com.mysql.jdbc.Driver"  
                connectionURL="jdbc:mysql://127.0.0.1:3306/crm?serverTimezone=GMT%2B8"  
                userId="root"  
                password="123456">  
        </jdbcConnection>  
  
        <!--  
             java类型处理器  
                用于处理DB中的类型到Java中的类型，默认使用JavaTypeResolverDefaultImpl；  
                注意一点，默认会先尝试使用Integer，Long，Short等来对应DECIMAL和NUMERIC数据类型；  
                true：使用 BigDecimal对应DECIMAL和NUMERIC数据类型  
                false：默认，把JDBC DECIMAL和NUMERIC类型解析为Integer  
        -->        <javaTypeResolver>  
            <property name="forceBigDecimals" value="false"/>  
        </javaTypeResolver>  
  
  
  
        <!-- 生成Model类存放位置 -->  
        <javaModelGenerator targetPackage="com.xxxx.crm.vo" targetProject="src/main/java">  
            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->  
            <property name="enableSubPackages" value="true"/>  
            <!-- 设置是否在getter方法中，对String类型字段调用trim()方法 -->  
            <property name="trimStrings" value="true"/>  
        </javaModelGenerator>  
  
  
        <!--生成映射文件存放位置-->  
        <sqlMapGenerator targetPackage="mappers" targetProject="src/main/resources">  
            <property name="enableSubPackages" value="true"/>  
        </sqlMapGenerator>  
  
  
        <!--生成Dao类存放位置-->  
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.xxxx.crm.dao" targetProject="src/main/java">  
            <property name="enableSubPackages" value="true"/>  
        </javaClientGenerator>  
  
  
  
        <table tableName="t_customer_serve" domainObjectName="CustomerServe"  
               enableCountByExample="false" enableUpdateByExample="false"  
               enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>  
  
    </context>  
</generatorConfiguration>
```

添加maven命令

![](../../markdown_img/Pasted%20image%2020230117173743.png)

运行后即可

## 用户模块

### 用户登录

- 整体思路：  
	1. 参数判断  
		用户姓名    非空判断  
		用户密码    非空判断  
	2. 通过用户名查询用户记录，返回用户对象  
	3. 判断用户对象是否为空  
	4. 如果用户对象不为空，则将前台传递的用户密码与数据库中的密码作比较  
	5. 判断密码是否正确  
	6. 如果密码正确，则登录成功，返回结果  

- Controller层 （控制层：接收请求、响应结果）  
	1. 通过形参接收客户端传递的参数  
	2. 调用业务逻辑层的登录方法，得到登录结果  
	3. 响应数据给客户端  

- Service层 （业务逻辑层：非空判断、条件判断等业务逻辑处理）  
	1. 参数判断，判断用户姓名、用户密码非空弄  
		如果参数为空，抛出异常（异常被控制层捕获并处理）            
	2. 调用数据访问层，通过用户名查询用户记录，返回用户对象  
	3. 判断用户对象是否为空  
		如果对象为空，抛出异常（异常被控制层捕获并处理）           
	4. 判断密码是否正确，比较客户端传递的用户密码与数据库中查询的用户对象中的用户密码  
		如果密码不相等，抛出异常（异常被控制层捕获并处理）           
	5. 如果密码正确，登录成功  

- Dao层 （数据访问层：数据库中增删改查操作）  
	通过用户名查询用户记录，返回用户对象  

**编写UserMapper**

```java
package com.xxxx.crm.dao;  
  
import com.xxxx.crm.base.BaseMapper;  
import com.xxxx.crm.vo.User;  

public interface UserMapper extends BaseMapper<User, Integer> {  
  
    // 通过用户名查询用户记录，返回用户对象  
    public User queryUserByName(String userName);  
}
```

**UserMapper.xml**

```xml
<!-- 通过用户名查询用户记录，返回用户对象 -->  
<select id="queryUserByName" parameterType="string" resultType="com.xxxx.crm.vo.User">  
  select      
	  <include refid="Base_Column_List"/>  
  from      
	  t_user  
  where      
	  user_name = #{userName}
</select>
```

**UserService**

```java
/**  
 * 用户登录
 * @param userName  
 * @param userPwd  
 * @return void  
 */
public UserModel userLogin(String userName,String userPwd) {  
	// 1. 参数判断，判断用户姓名、用户密码非空  
	checkLoginParams(userName, userPwd);  
  
	// 2. 调用数据访问层，通过用户名查询用户记录，返回用户对象  
	User user = userMapper.queryUserByName(userName);  
  
	// 3. 判断用户对象是否为空  
	AssertUtil.isTrue(user == null, "用户姓名不存在！");  
  
	// 4. 判断密码是否正确，比较客户端传递的用户密码与数据库中查询的用户对象中的用户密码  
	checkUserPwd(userPwd, user.getUserPwd());  
  
	// 返回构建用户对象  
	return buildUserInfo(user);  
}

/**  
 * 密码判断  
 *  先将客户端传递的密码加密，再与数据库中查询到的密码作比较  
 *  
 * @param userPwd  
 * @param pwd  
 * @return void  
 */
 private void checkUserPwd(String userPwd, String pwd) {  
	// 将客户端传递的密码加密  （工具类）
	userPwd = Md5Util.encode(userPwd);  
	// 判断密码是否相等  （工具类）
	AssertUtil.isTrue(!userPwd.equals(pwd), "用户密码不正确！");  
}

/**  
 * 参数判断  
 *  如果参数为空，抛出异常（异常被控制层捕获并处理）  
 *  
 * @param userName  
 * @param userPwd  
 * @return void  
 */
 private void checkLoginParams(String userName, String userPwd) {  
	// 验证用户姓名  
	AssertUtil.isTrue(StringUtils.isBlank(userName), "用户姓名不能为空！");  
	// 验证用户密码  
	AssertUtil.isTrue(StringUtils.isBlank(userPwd), "用户密码不能为空！");  
}

/**  
 * 构建需要返回给客户端的用户对象  
 *  
 * * @param user  
 * @return void  
 */
 private UserModel buildUserInfo(User user) {  
	UserModel userModel = new UserModel();  
	// userModel.setUserId(user.getId());  
	// 设置加密的用户ID  
	userModel.setUserIdStr(UserIDBase64.encoderUserID(user.getId()));  
	userModel.setUserName(user.getUserName());  
	userModel.setTrueName(user.getTrueName());  
	return userModel;  
}
```

**UserController**

```java
/**  
 * 用户登录  
 *  
 * @param userName  
 * @param userPwd  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("login")  
@ResponseBody  
public ResultInfo userLogin(String userName, String userPwd) {  
  
    ResultInfo resultInfo = new ResultInfo();  
	
	// 直接
    // 调用service层登录方法  
    UserModel userModel = userService.userLogin(userName, userPwd);  
    // 设置ResultInfo的result的值 （将数据返回给请求）  
    resultInfo.setResult(userModel);  

	// 异常处理
    // 通过try catch捕获service层的异常，如果service层抛出异常，则表示登录失败，否则登录失败  
    /*try{  
  
        // 调用service层登录方法  
        UserModel userModel = userService.userLogin(userName, userPwd);  
        // 设置ResultInfo的result的值 （将数据返回给请求）  
        resultInfo.setResult(userModel);  
    } catch (ParamsException p) {        resultInfo.setCode(p.getCode());        resultInfo.setMsg(p.getMsg());        p.printStackTrace();    } catch (Exception e) {        resultInfo.setCode(500);        resultInfo.setMsg("登录失败！");  
    }*/  
    return resultInfo;  
}
```

### 修改密码  
    
- Controller层  
	1. 通过形参接收前端传递的参数 （原始密码、新密码、确认密码）  
	2. 通过request对象，获取设置在cookie中的用户ID  
	3. 调用Service层修改密码的功能，得到ResultInfo对象  
	4. 返回ResultInfo对象  

- Service层  
	1. 接收四个参数 （用户ID、原始密码、新密码、确认密码）  
	2. 通过用户ID查询用户记录，返回用户对象  
	3. 参数校验  
		待更新用户记录是否存在 （用户对象是否为空）                
		判断原始密码是否为空                
		判断原始密码是否正确（查询的用户对象中的用户密码是否原始密码一致）                
		判断新密码是否为空                
		判断新密码是否与原始密码一致 （不允许新密码与原始密码）                
		判断确认密码是否为空                
		判断确认密码是否与新密码一致            
	4. 设置用户的新密码  
		需要将新密码通过指定算法进行加密（md5加密）  
	5. 执行更新操作，判断受影响的行数  

- Dao层  
	通过用户ID修改用户密码

在 `BaseMapper` 中已经有了通过 `id` 修改数据的方法

**UserService**

```java
/**  
 * 修改密码  
 *  
 * @param oldPwd  
 * @param newPwd  
 * @param repeatPwd  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updatePassWord(Integer userId, String oldPwd, String newPwd, String repeatPwd) {  
    // 通过用户ID查询用户记录，返回用户对象  
    User user = userMapper.selectByPrimaryKey(userId);  
    // 判断用户记录是否存在  
    AssertUtil.isTrue(null == user, "待更新记录不存在！");  
  
    // 参数校验  
    checkPasswordParams(user, oldPwd, newPwd, repeatPwd);  
  
    // 设置用户的新密码  
    user.setUserPwd(Md5Util.encode(newPwd));  
  
    // 执行更新，判断受影响的行数  
    AssertUtil.isTrue(userMapper.updateByPrimaryKeySelective(user) < 1, "修改密码失败！");  
  
}

/**  
 * 修改密码的参数校验  
 * 
 * @param user  
 * @param oldPwd  
 * @param newPwd  
 * @param repeatPwd  
 * @return void  
 */
private void checkPasswordParams(User user, String oldPwd, String newPwd, String repeatPwd) {  
    //  判断原始密码是否为空  
    AssertUtil.isTrue(StringUtils.isBlank(oldPwd), "原始密码不能为空！");  
    // 判断原始密码是否正确（查询的用户对象中的用户密码是否原始密码一致）  
    AssertUtil.isTrue(!user.getUserPwd().equals(Md5Util.encode(oldPwd)), "原始密码不正确！");  
  
    // 判断新密码是否为空  
    AssertUtil.isTrue(StringUtils.isBlank(newPwd), "新密码不能为空！");  
    // 判断新密码是否与原始密码一致 （不允许新密码与原始密码）  
    AssertUtil.isTrue(oldPwd.equals(newPwd),"新密码不能与原始密码相同！");  
  
    // 判断确认密码是否为空  
    AssertUtil.isTrue(StringUtils.isBlank(repeatPwd),"确认密码不能为空！");  
    // 判断确认密码是否与新密码一致  
    AssertUtil.isTrue(!newPwd.equals(repeatPwd), "确认密码与新密码不一致！");  
}
```

**UserController**

```java
/**  
 * 用户修改密码  
 *  
 * @param request  
 * @param oldPassword  
 * @param newPassword  
 * @param repeatPassword  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("updatePwd")  
@ResponseBody  
public ResultInfo updateUserPassword(HttpServletRequest request,  
         String oldPassword, String newPassword, String repeatPassword) {  
    ResultInfo resultInfo = new ResultInfo();  
  
    // 获取cookie中的userId  
    Integer userId = LoginUserUtil.releaseUserIdFromCookie(request);  
    // 调用Service层修改密码方法  
	userService.updatePassWord(userId, oldPassword, newPassword, repeatPassword);
   
    /* try {  
        // 获取cookie中的userId  
        Integer userId = LoginUserUtil.releaseUserIdFromCookie(request);        
        // 调用Service层修改密码方法  
        userService.updatePassWord(userId, oldPassword, newPassword, repeatPassword);  
    } catch (ParamsException p) {        
	    resultInfo.setCode(p.getCode());        
	    resultInfo.setMsg(p.getMsg());        
	    p.printStackTrace();    
	} catch (Exception e) {        
		resultInfo.setCode(500);        
		resultInfo.setMsg("修改密码失败！");  
        e.printStackTrace();    }*/  
    return resultInfo;  
}
```

### 退出功能

前端对 `cookie` 值进行清空（没有 `cookie` 就跳转 `login` 只有存在 `cookie` 才跳转 `home`）

### 记住我功能

前端对于 `cookie` 值的存储进行配置（7天失效，默认关闭浏览器失效）

