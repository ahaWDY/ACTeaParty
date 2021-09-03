# HTTP面试题

1. 如何用ajax原生实现一个post请求？

```js
var data = {
  city: city,
  area: area,
}
var data = 'city='+city+'area'+area;
var XHR = null;
if(window.XMLHttpRequest){
  //非IE内核
  XHR = new XMLHttpRequest();
}else if(window.ActiveXoject){
  //IE内核
  XHR = null;
}

if(XHR){
	XHR.open("POST","../plus/ddidy.php",true);
  XHR.onreadystatechange = function (){
    //readyState值说明
    //0.初始化，XHR对象已经创建，还未执行open
    //1.载入，已经调用open方法，但还未发送请求
    //2.载入完成，请求已经发送完成
    //3.交互，可以接收到部分数据
    
    //status值说明
    //200:成功
    //404:没有发现文件、查询或URL
    //500:服务器产生内部错误
    if(XHR.readyState == 4 && XHR.status == 200){
      //这里可以对返回的内容做处理
      //一般会返回json或者xml数据格式
      console.log(XHR.responseText);
      //主动释放，JS本身也会回收的
      XHR=null;
    }
  };
  XHR.setRequestHeader("Content-type","application/x-www-form-urlencoded");
  XHR.send(data);
}
```

参考：

https://blog.csdn.net/qiphon3650/article/details/77968224

https://blog.csdn.net/weixin_30607659/article/details/96514887