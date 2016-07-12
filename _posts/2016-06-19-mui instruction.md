---
layout: post
title:  "mui框架之弹出选择器(地址三级联动)"
date:   2016-07-12 09:41:58 +0800
categories: UAMES update
---

### mui框架

mui项目[github地址](https://github.com/dcloudio/mui)
mui首页[官方地址](http://dev.dcloud.net.cn/mui/)

页面中需导入以下文件：

```html
<link rel="stylesheet" type="text/css" href="__PUBLIC__/css/mui.min.css" />
<script src="__PUBLIC__/js/list/mui.min.js"></script>
<!-- 下面两个文件为选择器专用 -->
<script src="__PUBLIC__/js/list/mui.picker.js"></script>
<script src="__PUBLIC__/js/list/mui.poppicker.js"></script>
```

``mui.picker.js``和``mui.poppicker.js``已作为mui单独的插件发布，这是[地址](https://github.com/dcloudio/mui/tree/master/plugin/picker)

###### 下面是进行地址三级联动选择，或者单级列表选择时的代码示例

更多示例，请下载github上面的项目，在example文件夹中有具体示例代码~

```js
//三级联动
(function($, doc) {
  $.init();
  $.ready(function() {
    //级联示例
    var cityPicker3 = new $.PopPicker({
      layer: 3
    });
    var cityPicker2 = new $.PopPicker({
      layer: 1
    });
    streetData ="",tempArr='';

    var proData ="";
    $.ajax({
      url: ('/main/base/getAddress?' + new Date().getTime()),
      data:{pid:"0"},
      type: "POST",
      dataType:'json',
      async:false,
      success:function(data){
        proData = data;
      }
    });
    cityPicker3.setData(proData);

    var showCityPickerButton = doc.getElementById('showCityPicker3');
    showCityPickerButton.addEventListener('click', function(event) {
      cityPicker3.show(function(items) {
        showCityPickerButton.value =(items[0] || {}).text + " " + (items[1] || {}).text + " " + (items[2] || {}).text;
        tempArr[0] = items[0].id;
        tempArr[1] = items[1].id;
        tempArr[2] = items[2].id;
        showCityPickerButton.dataset.cityid =items[2].id;
        //返回 false 可以阻止选择框的关闭
        if(items[2].id > 0){
          $.ajax({
            url: ('/main/base/getAddress?' + new Date().getTime()),
            data:{pid:items[2].id},
            type: "POST",
            dataType:'json',
            async:false,
            success:function(data){
              //streetData = data;
              cityPicker2.setData(data);
            }
          });
        }
      });
    }, false);
    cityPicker2.setData(streetData);
  });
})(mui, document);

//单级列表
(function($, doc) {
  $.init();
  $.ready(function() {
    //级联示例
    var levelPicker = new $.PopPicker({
      layer: 1
    });

    var aData =[];
    $.ajax({
      url: ('/main/memberCard/getMemberCardLevel?' + new Date().getTime()),
      type: "GET",
      dataType:'json',
      async:false,
      success:function(data){
        if(data.code==1){
          var len = data.data.length;
          for(var i=0; i<len; i++){
            data.data[i].text = data.data[i].name;
          }
          aData = data.data;
        }
      }
    });
    levelPicker.setData(aData);

    var showLevelPickerButton = doc.getElementById('chooseLevel');
    showLevelPickerButton.addEventListener('click', function(event) {
      levelPicker.show(function(items) {
        showLevelPickerButton.value =(items[0] || {}).text;
        showLevelPickerButton.setAttribute("data-id", items[0].id);
        //返回 false 可以阻止选择框的关闭
      });
    }, false);
  });
})(mui, document);
```
