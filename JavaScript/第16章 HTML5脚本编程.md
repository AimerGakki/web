# 1.跨文档消息传递(cross-document messaging/XDN)
  
    指的是不同域的页面间传递消息
    postMessage()方法接收两个参数，一条消息和一个表示消息接收方来自哪个域的字符串
    如：
       var iframeWindow = document.getElementById("myframe").contentWindow;
       iframeWindow.postMessage('a message','http://www.wrox.com');
       
    接收：触发window对象的message事件  data : 数据,origin :发送域 ,source 代理window对象
      EventUtil.addHandler(window,"message",function(event){
      if(evnet.origin=="http://www.wrox.com"){
           //处理接收到的数据
           processMessage(event.data);
           
           // 可选,向来源窗口发送回执
           event.source.postMessage("Received","http://p2p.wrox.com");
          }
      
      })
       
### 代码展示

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8">
    <title>跨文档消息传输示例</title>
    <script src="EventUtil.js"></script>
    </head>
    <body>
    <iframe src="msg.html" width="400" height="400" onload="sendMsg();" id="iframe"></iframe>
    <div id="div1">
    </div>
    <script type="text/javascript">
    EventUtil.addHandler(window,"message",function(event){
        document.getElementById('div1').innerHTML='从event.origin='+event.origin+',那里来的消息event.data='+event.data;	   
    });
    //向id为iframe内嵌框架中发送一条消息
    function sendMsg(){
      //获取框架中的window
      var iframeWindow = document.getElementById("iframe").contentWindow;
      var information={"张三":10,"李四":20,"王五":30,"小明":40,"小白":50,"老五":60,"老周":67};
      var jsontext=JSON.stringify(information);
      setTimeout(function(){
      //iframe窗口加载完成，使用postMessage向iframe内发送消息
     //第一个参数为：消息内容
     //第二个参数为：接收消息的对象窗口url,一般与iframe的src一致
     iframeWindow.postMessage(jsontext,"http://localhost:8080/test/msg.html");},2000);
    }
    </script>
    </body>
    </html>
    
src.html
==

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8">
    <title>iframe内容</title>
    <script src="EventUtil.js"></script>
    <script type="text/javascript" >
    EventUtil.addHandler(window,"message",function(event){
     if(event.origin=="http://localhost:8080"){//注意origin是发送消息的文档所在的域。
    //接收从e.origin发来的消息e.data
       var obj = JSON.parse(event.data);
      aHtml = [];
      for (var key in obj) {
        var age = obj[key], str = '';
        switch(true) {
         case age<=10:
             str = key + '小朋友今年' + age + '岁。';
             break;
          case age<=20:
             str = key + '年青人今年' + age + '岁。';
             break;
         case age<=30:
             str = key + '中年人今年' + age + '岁。';
             break;
         case age<=40:
             str = key + '壮汉今年' + age + '岁。';
             break;
         case age<=50:
             str = key + '猛男今年' + age + '岁。';
             break;
          case age<=60:
             str = key + '快退休了，今年' + age + '岁。';
             break;
          default:
             str = key + '老年人今年' + age + '岁。';
             break;
       }
       aHtml.push(str);
    }
    document.body.innerHTML = '从event.origin=' + event.origin + ',发来的消息event.data=<br />' + aHtml.join('<br />'); 
    }else{
     alert("不可信任的消息源");
     }
       setTimeout(function() {
      //event.source即主页面的window对象，使用它向主页面发送消息
       event.source.postMessage('已经收到', event.origin);
     },2000);
   
    });
    </script>
    </head>
    <body>
    <div id="div1">
    </div>
    </body>
    </html>





