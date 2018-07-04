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
       
