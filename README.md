以下是我所用过的一些接口，希望对你有所帮助
# AKStream接口文档
运行StreamCtrl(管理程序)和MediaService（zlm）

若还没跑起来，请看zlm群文件（StreamNode编译运行文档）
(StreamCtrl接口文档)   http://设备IP:5800/swagger/index.html

(MediaService接口文档)  http://设备IP:6880/swagger/index.html

请确保云服务器5800和6880端口已开，否则访问不了swagger文档

### 如何接入一个国标摄像头
摄像头配置根据StreamCtrl/Config/gb28181.xml进行配置
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img1.png)
注册状态在线表示成功注册进去StreamNode，同时streamNode打印日志
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img2.png)

### 如何往一个ZLMediaKit上推流
摄像头接入成功之后，需要在streamCtrl上激活之后才能推流，需要在调用StreamCtrl接口文档激活摄像头，swagger文档找**到/MediaServer/ActivateSipCamera  激活sip网关自动添加的摄像头这个接口**

在调用之前，可以看到Request body需要我们自己填写一下参数，这些参数可以通过调用其他接口获得。请按截图中红色数组。1-1，2-2对应填写

1.**调用：/System/GetMediaServerList  获取流媒体服务器列表**
参数：无
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img3.png)
返回结果如图
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img4.png)

2.**调用/MediaServer/GetWaitForActiveCameraInstances 获取所有需要激活的摄像头实例**
参数：无
返回结果如图
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img5.png)

3.**调用：/MediaServer/ActivateSipCamera 激活sip网关自动添加的摄像头**,填入参数
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img6.png)

除1,2,3,4根据接口的值固定以外，其他的参数不固定，可暂时跟着我的填写。最好规范填写，这样数据库方便管理。Acticated为false时，激活摄像头不会自动推流

### 是否自动推流
- /SipGate/SetAutoPushStreamState设置Sip网关自动推流状态
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img7.png)

True：开启已激活的摄像头自动推流  false :关闭已激活的摄像头自动推流
- /SipGate/GetAutoPushStreamState 获取Sip网关自动推流状态
  
参数：无    返回：true 自动推流  false 不自动推流
- 若状态为false,可调用接口 /SipGate/LiveVideo 请求实时视频
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img8.png)
### MediaServer 信息查询与控制
- /MediaServer/GetConfig 获取流媒体配置信息

  传入参数：流媒体ID   返回：流媒体的配置
- /System/GetMediaServerList  获取流媒体服务器列表

  参数：无  返回值如图：
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img9.png)
- /MediaServer/CheckMediaServerRunning 检查流媒体服务是否正在运行
  参数：流媒体服务器ID （例如上面那个接口：6rt5K4YtkuiNv0FM）

返回：true 正在运行  false 不在运行
- /System/GetMediaServerInstance 获取一个流媒体服务的实例 

参数：流媒体服务器ID  
  - /MediaServer/StopMediaServer 关闭流媒体服务
  参数：流媒体服务器ID
