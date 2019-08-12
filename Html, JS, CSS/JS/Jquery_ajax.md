# Jquery_ajax

```javascript
$.ajax({
  contentType: 'application/x-www-form-urlencoded' // This is default. others, ex. applicaition/json,
  dataType: 'json',  // auto is default value, guessing type that the server will return
  url: ,
  data: ,
  success: function(){},
  error: function(){},
});
```



完整示例

```javascript
let post_data = {
  name: 'david',
  passwd: '123'
};
$.ajax({
  contentType: 'application/json',
  dataType: 'json',
  url: '/index',
  data: JSON.stringify(post_data),
  success: function(resp){console.log(resp)};
  error: function(error){console.log(error)};
});
```

当dataType=json，JS会将接收到的**字符串**按JSON格式进行解析，生成Obejct对象。