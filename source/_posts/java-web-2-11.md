---
title: Spring+SpringMVC+Hibernate实现投票/调查问卷网站
date: 2018-02-11 11:53:12
tags: java web
categories: Java Web
---

使用SSH架构（Spring+SpringMVC+Hibernate）实现了简单的调查问卷网站。最终效果如图：
![投票页面](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzYyNDA2NjQtMjQ2OTViZDhlZDFkMGZmMy5wbmc?x-oss-process=image/format,png)
![查看结果页面](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzYyNDA2NjQtZDA2ZjIyMjEzNTAwOGEwZS5wbmc?x-oss-process=image/format,png)

下面整理实现流程。

---
## 前言
#### 1.SSH架构
SSH是[MVC](https://baike.baidu.com/item/MVC)架构的一种实现。

Spring、SpringMVC、Hibernate各自用处分别是：
- Hibernate方便了对数据库的操作。一个对象映射一个表，省去了写SQL语句的繁琐，完成[数据持久化](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E6%8C%81%E4%B9%85%E5%8C%96)的任务。
- Spring方便了对象的创建和相互关联。比如网站启动时想要初始化的一些对象，可交给Spring管理。
- SpringMVC实现了MVC架构，使得结构清晰、分工明确。

（Spring和SpringMVC区别：Spring是IOC和AOP的容器框架，参考：[谈谈Spring中的IOC和AOP概念](http://blog.csdn.net/eson_15/article/details/51090040)）；SpringMVC是基于Spring实现的MVC Web框架）。
#### 2.Maven
Maven是一个项目管理工具，有一套标准的工程结构。其核心配置文件是pom.xml，描述了项目信息，依赖关系等。

由于Java项目中需要引入各种jar包，还存在版本差异，把这些依赖关系在pom.xml里面描述，maven就会自动从本地或远程仓库寻找依赖，不用再去一个个下载、拷贝jar包了。

例如，想引入springmvc框架，就在pom.xml中加入如下配置：
```
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>${spring.version}</version>
      </dependency
```
#### 3.代码结构
Java源码包含Model、DAO、Service、Controller四个包，其中：
- Model：存放数据模型
- DAO：实现直接操作Model的接口及方法，比如实现getPerson()
- Service：使用DAO提供的接口，实现项目需要用到的功能，比如实现getAllPersons()
- Controller：使用Service提供的功能，实现数据分发及页面展示。

工程结构如下：
![工程结构](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzYyNDA2NjQtMmJkNGEzNTExMmM2N2RiNC5wbmc?x-oss-process=image/format,png)
项目源码：https://github.com/zhang35/QuizWeb.git

---

## 开发环境

集成开发环境（IDE）：IntelliJ IDEA 2017.3.2 
本地服务器：Tomcat 9.0.2
数据库： MySQL 5.7
项目管理：Maven
操作系统：MacOS

---
## 开发步骤
### 1. 准备工作
- 如何使用IDEA创建web工程，参考：
 [使用IntelliJ IDEA开发java web](http://www.cnblogs.com/carsonzhu/p/5468223.html)

- 如何使用IDEA配置maven仓库，加快加载依赖包的速度，参考：
[IDEA配置maven(配置阿里云中央仓库)](http://www.cnblogs.com/sword-successful/p/6408281.html)

### 2. 搭建SSH项目
- 如何在IntelliJ与Maven的环境下搭建Spring+SpringMVC+Hibernate项目，参考：
[Spring-SpringMVC-Hibernate在IntelliJ与Maven的环境下搭建](http://blog.csdn.net/haluoluo211/article/details/52225074)

### 3. 实现投票功能
从操作流程出发，实现思路是：
#### 3.1 输入网址进入投票界面。（SpringMVC控制网址->页面的映射）
3.1.1 在web.xml中配置SpringMVC使之生效，web.xml：
```
   <servlet>
        <servlet-name>spring-dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>spring-dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```
3.1.2 在SpringMVC的配置文件servletname-servlet.xml中配置地址过滤规则，spring-dispatcher-servlet.xml：
```
    <context:component-scan base-package="web.quiz.controller" /><!-- 要扫描的包-->
    <mvc:annotation-driven />
    <!-- 解析网址，加前缀后缀，比如输入index时定位到/WEB-INF/jsp/index.jsp-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <!-- 不拦截静态资源，保证对js、css、jpg等的正常访问 -->
    <mvc:default-servlet-handler />
```

3.1.3 最后，在Controller中控制页面分发，QuizController.java：
```
    @RequestMapping(value = "vote", method = RequestMethod.GET)
    public String index() {
        return "vote";
    }
```

####3.2 获取问卷内容。（Hibernate+Spring从数据库中读取问卷信息，发送到前端页面）
3.2.1 首先使用Hibernate关联对象与数据库。Hibernate有配置xml和注解两种实现方式，本文使用注解。Person.java：
```
//@javax.persistence.Entity注解一个实体Bean。数据表中的行对应实例，列对应实例的属性
    @Entity
    public class Person {
	    @Id   //必须使用 @javax.persistence.Id 注解一个主键；
	    private String id;             //编号
	    private String name;            //姓名
    }
```
3.2.2 然后实现相应的DAO接口。PersonDAO.java、PersonDAOImpl.java：
```
    public interface PersonDAO {
        public List<Person> getAll();
    }
  
    @Repository //@Repository用于标注数据访问组件，即DAO组件；
    public class PersonDAOImpl implements PersonDAO{
    @Autowired //@Autowired可以对成员变量、方法和构造函数进行标注，来完成自动装配。默认按照类型进行装配。
    private SessionFactory sessionFactory;
      public List<Person> getAll() {
          Criteria criteria = sessionFactory.getCurrentSession().createCriteria(Person.class);
          return criteria.list();
      }
  }
```
3.2.3 PersonDAOImpl中要用的sessionFactory怎么来？什么是SessionFactory：
> SessionFactory接口负责初始化Hibernate。SessionFactory并不是轻量级的，因为一般情况下，一个项目通常只需要一个SessionFactory就够。

所以把SessionFactory放在Web初始化时候生成，使用Spring实现其自动装配。

首先在web.xml中配置Spring：
```
    <!--配置Spring-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:/META-INF/applicationContext.xml,
            classpath:/META-INF/spring-jdbc.xml
        </param-value>
    </context-param>
```

然后在Spring配置文件中配置Hibernate，spring-jdbc.xml：
```
       <!--自定义的hibernate.properties文件,下面${XXX}的内容来源-->
       <context:property-placeholder location="classpath:/META-INF/properties/hibernate.properties" />

       <!-- 使用C3P0数据源，MySQL数据库 -->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
             destroy-method="close">
              <!-- MySQL5 -->
              <property name="driverClass" value="${driverClassName}"></property>
              <property name="jdbcUrl" value="${url}"></property>
              <property name="user" value="${username}"></property>
              <property name="password" value="${password}"></property>
       </bean
       <!-- session工厂 -->
       <bean id="sessionFactory"
             class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
              <property name="dataSource" ref="dataSource" />
              <property name="packagesToScan" value="web.quiz.model" />
              <property name="hibernateProperties">
                     <props>
                            <prop key="hibernate.hbm2ddl.auto">${hibernate.hbm2ddl.auto}</prop>
                            <prop key="hibernate.dialect">${hibernate.dialect}</prop>
                            <prop key="hibernate.show_sql">${hibernate.show_sql}</prop>
                            <prop key="hibernate.format_sql">${hibernate.format_sql}</prop>
                     </props>
              </property>
       </bean>
        <!--hibernate的事务管理器-->
       <bean id="transactionManager"
             class="org.springframework.orm.hibernate4.HibernateTransactionManager">
              <property name="sessionFactory" ref="sessionFactory"></property>
       </bean>
```

在Spring的默认配置文件applicationContext.xml中配置扫描的包。这些包中有Spring注解为@Component的类，在使用注解配置的情况下，系统启动时会被自动扫描，并添加到bean工厂中去（省去了配置文件中写bean定义了）。applicationContext.xml：
```
    <context:component-scan base-package="web.quiz.service"/>
	<context:component-scan base-package="web.quiz.DAO"/>
```

至此，PersonDAOImpl中有了可用的sessionFactory，它的功能也能尽数实现了，如3.2.2中所示。

3.2.4 此时有了PersonDAO的实现，进一步封装成可直接用的服务。DBService.java 、DBServiceImpl.java：
```
public interface DBService{
    public List<Person> loadPersons();
}

@Service  //@Service用于标注业务层组件
@Transactional //@Transactional 可以作用于接口、接口方法、类以及类方法上。赋予其事务属性
public class DBServiceImpl implements DBService{
    @Autowired
    private PersonDAO personDAO;
    public List<Person> loadPersons(){
        return personDAO.getAll();
    }
}
```

至此，读写数据的功能有了。可以在Controller中调用服务读数据，QuizController.java：
```
@Controller
public class QuizController {
    @Resource  //与@Autowired等效，是JDK支持的注解，默认按照名称进行装配
    private DBService dbService;
    @PostConstruct  //使用@PostConstruct注释初始化方法。在Controller中，用@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器调用一次。
    private void initQuiz(){
        this.persons = dbService.loadPersons();
    }
}
```

3.2.5 把读取的内容发给前端JSP页面。本文使用了两种方法，一是传json回去，二是直接传对象回去。

**方法一**：传json到前端，用拼接字符串的方式生成页面。这种方法很笨，当时知识面太窄。不过用json传数据在做数据可视化时比较方便，所以也保留了这些代码。QuizController.java：
```
 @RequestMapping(value = "/loadPaper", method = RequestMethod.GET)
    @ResponseBody
    public byte[] loadPaper() throws IOException {
        ObjectMapper objectMapper = new ObjectMapper();
        String jsonString = objectMapper.writeValueAsString(this.quiz);
        byte[] b = jsonString.getBytes("UTF-8");        //解决传到前端后中文乱码问题
        return b;
    }
```
在前端用js解析json，生成页面。问卷页面vote.jsp：
```
function loadPage(){
            $.getJSON("loadPaper",function(data){  //获取问卷数据quiz,放入data中
                var names = data.names;
                 for (var i=0; i< names.length; i++){
                      var testDiv = '<div class="test">' + '<p class="name">' + (i+1) + ".&nbsp" + names[i] + '：</p>';
                      ...
                      $("form").append(testDiv);
                }
             }
```

**方法二**：借助ModelAndView对象，直接传对象到前端。配合JSP的特点，比较优雅高效。QuizController.java中：
```
//在方法的参数列表中添加形参 ModelMap map, spring 会自动创建ModelMap对象。
//然后调用map的put(key,value)或者addAttribute(key,value)将数据放入map中，spring会自动将数据存入request。
public ModelAndView check(HttpServletRequest request, HttpServletResponse response, ModelMap model){
  model.addAttribute("persons", persons);  //添加名为persons的对象
            return new ModelAndView("result");   //返回页面result.jsp
}
```
在前端使用JSTL <c:forEach>和EL表达式循环生成表格。显示成绩页面result.jsp：
```
        <c:forEach items="${persons}" var="person">
			<tr>
				<td>${person.id}</td>
				<td><a href="${person.id}/detail">${person.name}</a></td> <!--REST风格-->
				<td>${person.department}</td>
			</tr>
		</c:forEach>
```
（注意：使用EL表达式时，需加入<%@page isELIgnored="false" %>）

这里把每个人名作为一个超链接，点进去显示其测评结果。${person.id}/detail会变成如1/detail这样的地址。该地址在QuizController.java中这么解析：
```
@RequestMapping(value = "/{id}/detail", method = RequestMethod.GET)
    public ModelAndView detail(HttpServletRequest request, ModelMap model, @PathVariable String id) {
        Result result = dbService.getResultByID(id);
        String name = result.getName();
        model.addAttribute("name", name);
        return new ModelAndView("detail");
    }
```
3.2.6 扩展知识
上述传参方式叫REST( Representational State Transfer)风格。
关于SpringMVC应用REST风格，参考：[Spring MVC 实现增删改查 - CSDN博客](https://www.baidu.com/link?url=d-Rg_gxkKlAH95hnCYk2_m7EqFvgBpt9a3ZiWCFLgiN0fTCCqs4KH0dAW0MI8b0_MdvigqzLJSI0ysAufrE0uK&wd=&eqid=b83da03c00046a25000000025a67d318)
关于其它URL传参方式，参考： [SpringMVC之@RequestParam @PathVariable对比](http://www.cnblogs.com/wangchuanfu/p/5913310.html)
关于SpringMVC中各种常用传值方法，参考：[springMVC 将controller中数据传递到jsp页面](http://blog.csdn.net/sunshine__me/article/details/49494545)
#### 3.3 填写问卷，点提交。(Controller接收前端表单，结果写入数据库)
主要是前端传值到后端的问题。
在vote.jsp中：
```
<form method="POST" action="submit">
...
</form>
```
在QuizController.java中：
```
 @RequestMapping(value = "/submit", method = RequestMethod.POST)
    public String submit(HttpServletRequest request, HttpServletResponse response) {
        Enumeration<String> enu = request.getParameterNames();        //获得表单中所有值
        while (enu.hasMoreElements()) {
            String paraName = (String) enu.nextElement();
            String value = request.getParameter(paraName);   //按照表单中的顺序，一个个接收值
            ...
            dbService.saveOrUpdateResult(result); //将成绩写入数据库中
        }
    }
```
#### 3.4 管理员登录，查看结果。(身份验证后，JSP展示结果)
提供管理员登录页面login.jsp，登录后进入参评人列表页面result.jsp；点击参评人超链接，进入详细成绩页面detail.jsp，统计分析个人得票结果。

到这里已经没有什么难度，不再写了。

---
## 一点心得
### 1. hibernate如何将数组成员对应到数据库

对于Question对象，成员id、title都能自动对应数据库中表的一列，而options作为一个List就带来很多麻烦。

**解决方法：把List<String>拼接成一个String保存**，当使用时，按照约定规则拆分即可：
```
    String[] options = questions[i].options.split("#"); //自行约定，这里用#分隔字符
```
成员变为：
```
	private String options;
```
完美！

---
## 结语
终于搭好Java SSH这个架子，实现了基本功能，但不想再继续写了。Web技术迭代太快，后面去尝试下其它技术。