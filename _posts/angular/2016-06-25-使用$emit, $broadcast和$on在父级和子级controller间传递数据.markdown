---
layout: page
title:  "使用$emit, $broadcast和$on在父级和子级controller间传递数据"
---

#### AngularJS的学习--$on、$emit和$broadcast的使用  
使用$emit, $broadcast和$on在父级和子级controller间传递数据
> AngularJS中的作用域有一个非常有层次和嵌套分明的结构。其中它们都有一个主要的$rootScope(也就说对应的Angular应用或者ng-app)，然后其他所有的作用域部分都是继承自这个$rootScope的，或者说都是嵌套在主作用域下面的。很多时候，你会发现这些作用域不会共享变量或者说都不会从另一个原型继承什么。

> 那么在这种情况下，如何在作用域之间通信呢？其中一个选择就是在应用程序作用域之中创建一个单例服务，然后通过这个服务处理所有子作用域的通信。

> 在AngularJS中还有另外一个选择：通过作用域中的事件处理通信。但是这种方法有一些限制；例如，你并不能广泛的将事件传播到所有监控的作用域中。你必须选择是否与父级作用域或者子作用域通信。

$on、$emit和$broadcast使得event、data在controller之间的传递变的简单。  
* $emit只能向parent controller传递event与data  
* $broadcast只能向child controller传递event与data  
* $on用于接收event与data  

例子如下

html代码  

```html
<div ng-controller="ParentCtrl">                <!--父级-->
    <div ng-controller="SelfCtrl">              <!--自己-->
        <a ng-click="click()">click me</a>
        <div ng-controller="ChildCtrl"></div>   <!--子级-->
    </div>
    <div ng-controller="BroCtrl"></div>         <!--平级-->
</div>
```

js代码

```javascript
app.controller('SelfCtrl', function($scope) {
    $scope.click = function () {
        $scope.$broadcast('to-child', 'child');
        $scope.$emit('to-parent', 'parent');
    }
});

app.controller('ParentCtrl', function($scope) {
    $scope.$on('to-parent', function(event,data) {
        console.log('ParentCtrl', data);       //父级能得到值
    });
    $scope.$on('to-child', function(event,data) {
        console.log('ParentCtrl', data);       //子级得不到值
    });
});

app.controller('ChildCtrl', function($scope){
    $scope.$on('to-child', function(event,data) {
        console.log('ChildCtrl', data);         //子级能得到值
    });
    $scope.$on('to-parent', function(event,data) {
        console.log('ChildCtrl', data);         //父级得不到值
    });
});

app.controller('BroCtrl', function($scope){  
    $scope.$on('to-parent', function(event,data) {  
        console.log('BroCtrl', data);          //平级得不到值  
    });  
    $scope.$on('to-child', function(event,data) {  
        console.log('BroCtrl', data);          //平级得不到值  
    });  
});
```

最终结果

ParentCtrl parent

ChildCtrl child

$emit和$broadcast可以传多个参数，$on也可以接收多个参数。  
在$on的方法中的event事件参数，其对象的属性和方法如下  

| 事件属性       | 目的      |
| ------------- |:-------------:|
|event.targetScope|	发出或者传播原始事件的作用域|
|event.currentScope|目前正在处理的事件的作用域|
|event.name|事件名称|
|event.stopPropagation()|	一个防止事件进一步传播(冒泡/捕获)的函数(这只适用于使用`$emit`发出的事件)|
|event.preventDefault()	这个方法实际上不会做什么事，但是会设置`defaultPrevented`为true|event.preventDefault()	这个方法实际上不会做什么事，但是会设置`defaultPrevented`为true。直到事件监听器的实现者采取行动之前它才会检查`defaultPrevented`的值|
|event.defaultPrevented|	如果调用了`preventDefault`则为true|  

service在不同controller中通信要方便许多~~

转于[博客园](http://www.cnblogs.com/CraryPrimitiveMan/p/3679552.html)
