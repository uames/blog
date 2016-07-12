---
layout: post
title:  "阿里手机H5应用SUI库首次使用<br/>(日期选择)"
---

### 阿里手机H5应用SUI库首次使用(日期选择)
* 用一些组件可以加快开发的速度，就如这里，使用了它日期选择的主机，界面挺漂亮的~
就是开发的时候，按照文档发现有点小问题，于是把例子copy到本地，精简之后，发现下面的代码，``<div class="page"><div class="content"></div></div>``不能再减了，另外sm.js必须要在页面加载后再加载，最后要$.init()启动组件。

* 另外，你或许会发现，这里用了zepto而不是jquery，如果项目是用jquery的。在github上有个[issues](https://github.com/sdc-alibaba/SUI-Mobile/issues/143)专门讨论这个问题。

直接复制下面的代码在浏览器打开就能看到效果啦~
或者打开[这个链接](http://m.sui.taobao.org/demos/calendar/)

```html
<html>
 <head>
  <meta charset="utf-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="initial-scale=1, maximum-scale=1" />
  <link rel="stylesheet" href="http://g.alicdn.com/msui/sm/0.6.2/css/sm.css">
  <script type='text/javascript' src='http://g.alicdn.com/sj/lib/zepto/zepto.js' charset='utf-8'></script>
</head>
 <body>
   <div class="page">
    <div class="content">
      <input type="text" id="birthday" data-toggle="date"/>
    </div>
   </div>
  <script type='text/javascript' src='http://g.alicdn.com/msui/sm/0.6.2/js/sm.js' charset='utf-8'></script>
  <script type="text/javascript">
      $.init();
  </script>
 </body>
</html>
```
![配图]({{ "/public/img/rili.png" | prepend: site.baseurl }})
