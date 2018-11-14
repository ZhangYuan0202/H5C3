### 网络接口

​      `ononline`:当网络连通时触发

​      `onoffline`:当网络断开时触发

### 全屏接口

#####     全屏操作的主要方法和属性

​      1.`requestFullScreen()`: 开启全屏显示

​          注意：不同浏览器需要加不同的前缀：

​                        Chrome ：加webkit           firefox：moz      ie：ms           opera：o

​      2.`cancelFullScreen()`:退出全屏显示

​           注意：退出全屏只能通过document来实现

​      3.`fullScreenElement`:是否是全屏状态

​               也只能用document进行判断

  代码演示：

```javascript
<div>
    <img src="../images/l1.jpg" alt="">
    <input type="button" id="full" value="全屏">
    <input type="button" id="cancelFull" value="退出全屏">
    <input type="button" id="isFull" value="是否全屏">
</div>
<script>
   window.onload=function () {
       var div=document.querySelector("div");
       //点击注册全屏事件
       document.querySelector("#full").onclick=function () {
           if (div.requestFullScreen){
               div.requestFullScreen();
           } else if (div.webkitRequestFullScreen){
               div.webkitRequestFullScreen();
           }else if (div.mozRequestFullScreen){
               div.mozRequsetFullScreen();
           }else if (div.msRequsetFullScreen){
               div.msRequsetFullScreen();
           }else if (div.oRequsetFullScreen){
               div.oRequsetFullScreen();
           }
       }
```

退出全屏：

```javascript
//点击注册退出全屏事件
       document.querySelector("#cancelFull").onclick=function () {
           if (document.cancelFullScreen){
               document.cancelFullScreen();
           } else if (document.webkitCancelFullScreen){
               document.webkitCancelFullScreen();
           }else if (document.mozCancelFullScreen){
               document.mozCancelFullScreen();
           }else if (document.msCancelFullScreen){
               document.msCancelFullScreen();
           }else if (document.oCancelFullScreen){
               document.oCancelFullScreen();
           }
       }
```

判断是否全屏：

```javascript
 //判断是否为全屏
       document.querySelector("#isFull").onclick=function () {
           if(document.fullscreenElement||document.webkitFullscreenElement||document.mozFullscreenElement||document.msFullscreenElement||document.oFullscreenElement){
               alert("true");
           }else{
               alert("false")
           }
       }
   }
```

整体实现效果：

![ijZ7ZQ.gif](https://s1.ax1x.com/2018/11/14/ijZ7ZQ.gif)

[](https://imgchr.com/i/ijZ7ZQ)

### FileReader的使用

#####    **FileReader**:读取文件内容* 

​    1.readAsText():读取文本文件(可以使用Txt打开的文件)，返回文本字符串，默认编码是UTF-8* 

​     2.readAsBinaryString():读取任意类型的文件。返回二进制字符串。这个方法不是用来读取文件展示给用户看，而是存储文件。例如：读取文件的内容，获取二进制数据，传递给后台，后台接收了数据之后，再将数据存储* 

​    3.readAsDataURL():读取文件获取一段以data开头的字符串，这段字符串的本质就是DataURL.DataURL是一种将文件(这个文件一般就是指图像或者能够嵌入到文档的文件格式)嵌入到文档的方案。DataURL是将资源转换为base64编码的字符串形式，并且将这些内容直接存储在url中>>优化网站的加载速度和执行效率。

#####  即时预览  

   代码示例：

```javascript
<img src="" alt="">
<script>
function getFileContent() {
    var reader=new FileReader();
    var file=document.querySelector("#myFile").files;
      reader.readAsDataURL(file[0]);
      reader.onload=function () {
        console.log(reader.result);
         document.querySelector("img").src=reader.result;
     }
   
}
</script>
```

显示效果：

![ijM6Z8.gif](https://s1.ax1x.com/2018/11/14/ijM6Z8.gif)

### 拖拽接口的使用

​        在H5中如果想拖拽元素，就必须为元素添加 draggable=“true” 。图片和超链接就可以拖拽。

####    拖拽的事件

#####            1.应用于被拖拽元素的事件

- ondrag         应用于拖拽元素，整个拖拽过程都会调用--持续    
- ondragstart    应用于拖拽元素，当拖拽开始时调用 
- ondragleave    应用于拖拽元素，当鼠标离开拖拽元素时调用 
- ondragend    应用于拖拽元素，当拖拽结束时调用

#####             2.应用于目标元素的事件

- ondragenter   应用于目标元素，当拖拽元素进入时调用
- ondragover    应用于目标元素，当停留在目标元素上时调用
- ondrop       应用于目标元素，当在目标元素上松开鼠标时调用
- ondragleave    应用于目标元素，当鼠标离开目标元素时调用

##### 拖拽案例

```javascript
<div class="div1" id="div1">
    <!--在h5中，如果想拖拽元素，就必须为元素添加draggable="true". 图片和超链接默认就可以拖拽-->
    <p id="pe" draggable="true">试着把我拖过去</p>
    <p id="pe1" draggable="true">试着也把我拖过去</p>
</div>
<div class="div2" id="div2"></div>
<script>
    var p1=document.querySelector("#pe");
    var p2=document.querySelector("#pe1");
    var div1=document.querySelector("#div1");
    var div2=document.querySelector("#div2");
    div2.ondragover=function(e){
        e.preventDefault();
    }
    div2.ondrop=function () {
        div2.appendChild(p1);
        // div2.appendChild(p2)

    }
    document.ondragover=function(e){
        e.preventDefault();
    }
    document.ondrop=function (e) {
        var id=e.dataTransfer.getData("text/html");
        e.target.appendChild(document.getElementById(id));
    }
    document.ondragstart=function (e) {
        e.target.opacity=0.3;
        e.target.parentNode.style.borderWidth='5px';
        e.dataTransfer.setData("text/html",e.target.id);
    }
    document.ondragend=function (e) {
        e.target.opacity=1;
        e.target.parentNode.style.borderWidth='1px';
    }
</script>
```

   **注意**：浏览器默认会阻止ondrop事件：我们必须在ondragover中阻止浏览器的默认行为（e.preventDefault）。

   显示效果：

![ijYrEq.gif](https://s1.ax1x.com/2018/11/14/ijYrEq.gif)

### 地理定位接口

     ```javascript
<div id="demo" class="de"></div>
<script>
    var x=document.getElementById("demo");
    function getLocation()
    {
        /*能力测试*/
        if (navigator.geolocation)
        {
            /*1.获取地理信息成功之后的回调
            * 2.获取地理信息失败之后的回调
            * 3.调整获取当前地进信息的方式*/
            //navigator.geolocation.getCurrentPosition(success,error,option);
            /*option:可以设置获取数据的方式
            * enableHighAccuracy:true/false:是否使用高精度
            * timeout:设置超时时间，单位ms
            * maximumAge:可以设置浏览器重新获取地理信息的时间间隔，单位是ms*/
            navigator.geolocation.getCurrentPosition(showPosition,showError,{
                /*enableHighAccuracy:true,
                timeout:3000*/
            });
        }
        else{
            x.innerHTML="Geolocation is not supported by this browser.";
        }
    }
    /*成功获取定位之后的回调*/
    /*如果获取地理信息成功，会将获取到的地理信息传递给成功之后的回调*/
    // position.coords.latitude 纬度
    // position.coords.longitude 经度
    // position.coords.accuracy 精度
    // position.coords.altitude 海拔高度
    function showPosition(position)
    {
        x.innerHTML="Latitude: " + position.coords.latitude +
                "<br />Longitude: " + position.coords.longitude;
    }
    /*获取定位失败之后的回调*/
    function showError(error)
    {
        switch(error.code)
        {
            case error.PERMISSION_DENIED:
                x.innerHTML="User denied the request for Geolocation."
                break;
            case error.POSITION_UNAVAILABLE:
                x.innerHTML="Location information is unavailable."
                break;
            case error.TIMEOUT:
                x.innerHTML="The request to get user location timed out."
                break;
            case error.UNKNOWN_ERROR:
                x.innerHTML="An unknown error occurred."
                break;
        }
    }
    getLocation();
</script>
     ```

### web 存储

​        1.`sessionStorage`

​              sessionStorage的使用：

​            1.存储数据到本地。存储的容量5mb左右。

​            2.这个数据本质是存储在当前页面的内存中-意味着其它页面和浏览器无法获取数据

​            3.它的生命周期为关闭当前页面，关闭页面，数据会自动清除     

​         setItem(key,value):存储数据，以键值对的方式存储

​         getItem(key):获取数据，通过指定名称的key获取对应的value值

​         removeItem(key):删除数据，通过指定名称key删除对应的值

​         clear():清空所有存储的内容

代码演示：

```javascript
<input type="text" id="userName"><br>
<input type="button" value="设置数据" id="setData">
<input type="button" value="获取数据" id="getData">
<input type="button" value="删除数据" id="removeData">
<script>
    /*存储数据*/
    document.querySelector("#setData").onclick=function(){
        /*获取用户名*/
        var name=document.querySelector("#userName").value;
        /*存储数据*/
        window.sessionStorage.setItem("userName",name);
    }
    /*获取数据*/
    document.querySelector("#getData").onclick=function(){
        /*如果找不到对应名称的key,那么就会获取null*/
        var name=window.sessionStorage.getItem("userName");
        alert(name);
    }
    /*删除数据*/
    document.querySelector("#removeData").onclick=function(){
        /*在删除的时候如果key值错误，不会报错，但是也不会删除数据*/
        window.sessionStorage.removeItem("userName1");
    }
</script>
```

​      2.`localStorage`

​       localStorage的使用：

​          1.存储的内容大概20mb

​           2.不同浏览器不能共享数据。但是在同一个浏览器的不同窗口中可以共享数据

​            3.永久生效，它的数据是存储在硬盘上，并不会随着页面或者浏览器的关闭而清除.如果想清除，必须手动清除

 代码演示：

```javascript
<input type="text" id="userName"><br>
<input type="button" value="设置数据" id="setData">
<input type="button" value="获取数据" id="getData">
<input type="button" value="删除数据" id="removeData">

<script>
    document.querySelector("#setData").onclick=function(){
        var name=document.querySelector("#userName").value;
        /*使用localStorage存储数据*/
        window.localStorage.setItem("userName",name);
    }
    /*获取数据*/
    document.querySelector("#getData").onclick=function(){
        var name=window.localStorage.getItem("userName");
        alert(name);
    }
    /*清除数据*/
    document.querySelector("#removeData").onclick=function(){
        window.localStorage.removeItem("userName");
    }
</script>
```

### 应用缓存

​     