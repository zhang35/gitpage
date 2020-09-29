---
title: AngularJS学习笔记
date: 2018-07-23 08:11:21
tags: AngularJS web
categories: 前端开发
description: 总结AngularJS基础
---

做绩效考评网站前端，用到angular.js，很强大。

## 简介

> AngularJS 是一个 JavaScript 框架。

> AngularJS 使得开发现代的单一页面应用程序（SPAs：Single Page Applications）变得更加容易。

- AngularJS 把应用程序数据绑定到 HTML 元素。
- AngularJS 可以克隆和重复 HTML 元素。
- AngularJS 可以隐藏和显示 HTML 元素。
- AngularJS 可以在 HTML 元素"背后"添加代码。
- AngularJS 支持输入验证。

## 数据双向绑定

```
<div ng-app="myApp" ng-controller="myCtrl">
    名字: <input ng-model="name">
</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.name = "John Doe";
});
</script>
```

view和controller中的值绑定后，改变一方，另一方实时变化。

## ng-repeat循环元素
基本用法：x in records:
```
    <tr ng-repeat="x in records">
        <td>{{x.Name}}</td>
        <td>{{x.Country}}</td> 
    </tr>
```

同时输出key和value，(x, y) in myObj：
```

<table ng-controller="myCtrl" border="1">
    <tr ng-repeat="(x, y) in myObj">
        <td>{{x}}</td>
        <td>{{y}}</td> 
    </tr>
</table>

<script>
var app = angular.module("myApp", []);
app.controller("myCtrl", function($scope) {
    $scope.myObj = {
        "Name" : "Alfreds Futterkiste",
        "Country" : "Germany",
        "City" : "Berlin"
    }
});
</script>
```
## ng-options下拉选项
参考：[angular指令笔记*ng-options*的使用方法_AngularJS_脚本之家](https://www.baidu.com/link?url=_XHaqFuLPeXar6Kxwte4sMXDIJtFXcnqdx6O71T8u8_Z57DGHDm-7NoVF2j1CeQ7bZb6Zqh5tCutIJ192EdwqK&wd=&eqid=9621fda7000156d4000000035b3ae580)

```
<select ng-options="c.id as c.city for c in obj1" ng-model="selectedCity" ng-change="SelecteCity()">
```
这里  c.id as c.city for c in obj  我们使用 obj 对象的 id作为select的value，使用obj 的city 作为 select 的label。

本文的jade代码：

```
select.form-control(ng-options='item.departmentId as item.departmentName for item in relations', ng-model='search.departmentId')
```
relations.departmentName为显示的label，relations.departmentId为value，绑定的model是search.departmentId。

（relations是Controller中定义的对象）


## $scope作用域问题：

例如，前端指定ng-change='loadData'：

```
select.form-control(ng-options='item.id as item.username for item in persons', ng-model='search.personId',ng-change='loadData')
```

在js中定义loadData()：
```
var loadData = function () {
    $http(buildParam())
        .then(function (response) {
            if (response.data.status === 200) {
                $scope.items = response.data.data;
            } else {
                $.notify(response.data.message, 'danger');
            }
        },
        function (x) {
            $.notify('服务器出了点问题，我们正在处理', 'danger');
        });
};
```
当选项变更时，是无法触发loadData的，因为前端能交互的作用域都在$scope里。需要添加：

```
$scope.loadData = loadData();
```

或者直接如下定义：
```
$scope.loadData = function(){
    ……
}
```