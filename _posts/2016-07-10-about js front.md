---
layout: post
title:  "JS前端的构思"
author: "Uames"
categories: Uames

# data & key 获取的基础JS代码
# var e=new Date(),m=e.getMonth()+1,m=m>9?m:'0'+m,d=e.getDate(),d=d>9?d:'0'+d,h=e.getHours(),h=h>9?h:'0'+h,i=e.getMinutes(),i=i>9?i:'0'+i,s=e.getSeconds(),s=s>9?s:'0'+s;
date:   2016-07-12 09:41:58 +0800
# date 在浏览器中运行下面的代码获取 date 值
# console.log(e.getFullYear()+'-'+m+'-'+d+' '+h+':'+i+':'+s+' +0800');
key: 20160712094158
# key 多说评论的唯一值
# 在浏览器中运行下面这句代码获取key值，key值确定后不可修改
# console.log(''+e.getFullYear()+m+d+h+i+s);
---

- 按功能模块化
- 统一资源路径加载
- 配置文件

项目使用require + jquery，另外不间断引入一些其它的插件和组件如layer,mui,sui.  
另外自己封装了一些功能性的js，用于复用大大加快了开发效率，并且获得了更好的代码结构。  
前端使用单独的配置文件：config.js

```javascript
var config = {
	serviceHost : 'http://easy.com',
	srcUrl: 'http://m.test.taozhenwang.com:800/public/201504',
	pubUrl: '/js/lib/public',
	ngMenu: 'http://cdn.bootcss.com/angular.js/1.5.7/',
	jqUrl: 'http://cdn.bootcss.com/jquery/1.11.1/jquery.min',
	getUrl: function(url){
		return config.serviceHost + url;
	},
	init: function(){
		config.pubUrl = config.srcUrl + config.pubUrl;
	}
}
config.init();
require.config({
	paths: {
		'ng': config.ngMenu + 'angular.min',
		'jq': [config.jqUrl, config.srcUrl + '/js/lib/jquery-1.11.1.min'],
		'pub': config.srcUrl+'/js/lib/public'
	}
});
```

这里是copy了新项目（使用了angular）的配置文件过来配置的。  
页面用require引导加载的js文件中，先用一句代码  
``require.config({paths: {'config': '../config'}});``  
把配置文件加载进来，然后下面：

```javascript
require(['config'], function (){
	require(['jq'],function(){
			require(['pub/public'],function(){
        //尽情书写你的代码吧
			});
	});
});
```

将配置文件作为最外层的依赖模块``require(['config'], function (){});``，加载完后，配置文件中require的配置即生效，再依次加载config.js中配置的jquery和public目录下的public.js.
  （关于require.js的目录配置和文件配置，建议通读一次[require文档](http://www.requirejs.cn/)，以期对其它特性也有一定的了解）

---

#### 1. 按功能模块化，封装包括服务的功能，封装的时候要注意可扩展性。
* 先已有的有phone-email.js
