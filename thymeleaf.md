---
title: "Thymeleaf"
date: 2019-12-28T18:13:15+08:00
draft: false
keywords: [thymeleaf简介：]
description: ""
tags: [thymeleaf简介：]
categories: [thymeleaf简介：]
author: ""
---
Thymeleaf的使用
<!--more-->
***
## thymeleaf简介：
thymeleaf是一种Java模板引擎，那何为模板引擎呢？模板引擎就是为了使用户页面和业务数据相互分离而出现的，
将从后台返回的数据生成特定的格式的文档，这里说的特定格式一般都指HTML文档

## 引入约束
<html xmlns:th="http://www.thymeleaf.org">
## thymeleaf标准方言:
* 变量表达式：${...}
	例如前端接收一个user,想取出user的name属性
	<span th:text="${user.name}">

* 消息表达式：#{...}
	也称为文本外部化、国际化或i18n.
	文字国际化表达式允许我们从一个外部文件获取区域文字信息(.properties)，用 Key 索引 Value
	 <th th:text="#{header.address.city}">...</th>  
 	 <th th:text="#{header.address.country}">...</th>  

* 选择表达式：*{...}
	与变量表达式的区别：选择表达式是在 当前选择的对象上执行（父标签的值）。
	<form action="/users" th:action="@{/users}" method="POST" th:object="${userModel.user}">
		<input type="hidden" name="id" th:value="*{id}">
	</form>
	在此处*{id}与${userModel.user.id}效果一样。
	* 由 th:object 属性定义

* 链接表达式：@{...}
	url可以是相对的，也可以是绝对的，URL还可以设置参数。
	<a th:href="@{.../users/list}">...</a>
	<a th:href="@{http://www.baidu.com}">...</a>
	<a th:href="@{/order/details(id=${orderId})}">...</a>
		
* 分段表达式：th:insert 、th:replace 、th:include
	就相当插入。这三个的区别：
	现有一个片段如下：


## 表达式支持的语法
	* 文体文字	'one text'
	* 数字文本	0，1，2
	* 布尔文本	true，false
	* 空			null
	* 文字标记	one，sometext
	* 字符串拼接	+
	* 算术运算	+, -, *, /, %,-(单目)
	* 布尔运算	and, or
	* 布尔否定	！，not
	* 比较		>, <, >=, <= (gt, lt, ge, le)
	* 等值运算	==, != (eq, ne)
	* 条件运算符	If-then:(if) ? (then)
				If-then-else:(if) ? (then) : (else)
				Default: (value) ?:(defaultvalue)
				就是三目运算符   el ? el : el
				

## th标签
	* th:id 		替换id
	* th:text
	* th:utext	支持html的文本替换
	* th:object	替换对象
	* th:value	
	* th:with 	变量赋值运算 <div th:with="isEven=${prodStat.count}%2==0"></div> 就是为标签添加属性并赋值
	* th:style
	* th:onclick
	* th:each		相当于Java的foreach	<tr th:each="user : ${userList}">
										 <td th:text="${user.id}"></td>
										 <td th:text="${user.email}"></td>
									</tr>
	* th:if
	* th:unless	和if相反	<a th:href="@{/login}" th:unless=${session.user != null}>Login</a>
	* th:href  th:href="@{/Controller/behavior(param1=1,param2=${person.id})}"
	
	
		
	* th:swith th:case 多路选择	<div th:switch="${user.role}"> 
								<p th:case="'admin'">User is an administrator</p>
	* th:fragment	布局标签，定义一个代码片段，方便其它地方引用 
							<div th:fragment="header">
							   <h1>Thymeleaf in action</h1>
							   <a href="/users" >首页</a>
							</div>
				在其他页面直接这样引用就行：
						<div th:replace="~{fragments/header :: header}"></div>

	* th:include  布局标签,替换内容到引入的文件 <head th:include="layout :: htmlhead" th:with="title='xx'"></head>
	* th:replace  布局标签,替换整个标签到引入的文件 <div th:replace="fragments/header :: title"></div>
	* th:selected selected选择框 是否选中
	* th:src
	* th:inline   定义js脚本可以使用变量 <script type="text/javascript" th:inline="javascript">
	* th:action   表单提交的地址
	* th:remove 	删除某个属性
							<tr th:remove="all"> 
							1.all:删除包含标签和所有的孩子。
							2.body:不删除自己,但删除其所有的孩子。
							3.tag:包含标记的删除,但不删除它的孩子。
							4.all-but-first:删除所有包含标签的孩子,除了第一个。
							5.none:什么也不做。这个值是有用的动态评估。
							
							
	* th:attr 	设置标签属性,多个属性可以用逗号隔开 th:attr="src=@{/image/aa.jpg},title=#{logo}"
	
	注:多个th标签同时存在,其生效顺序为
		include,each,if/unless/switch/case,with,attr/attrprepend/attrappend,value/href,src ,etc,text/utext,fragment,remove

### Thymeleaf中href与 th:href的区别		
>语法格式如下：

`<a th:href="@{/channel/page/add}">添加渠道 </a>`

`<a href="/channel/page/add">添加渠道 </a>`  
在默认项目路径为空时，打Jar包单独运行时。二者效果一致。

在使用Maven内嵌Tomcat或打War包部署到Servlet容器，或者在项目内执行App启动类，且有配置项目路径时。

二者区别如下：  

href始终从端口开始作为根路径，如http://localhost:8080/channel/page/add  

th:href会寻找项目路径作为根路径，如http://localhost:8080/dx/channel/page/add  

href也可以在相对路径前, 先获取绝对路径
							
