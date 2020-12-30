以下是我所用过的一些接口，希望对你有所帮助，图片需要翻墙才能看到

下载地址：https://github.com/getlantern/lantern

根据自己的系统装，装完之后运行软件再打开此链接，就能看到图片了

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
- /MediaServer/StartMediaServer 启动流媒体服务
  
  参数：流媒体服务器ID
- /MediaServer/RestartMediaServer 重启流媒体服务

  参数：流媒体服务器ID  
  ### 视频流
- /MediaServer/AddFFmpegProxy 启动一个ffmpeg代理流
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img10.png)
- /MediaServer/CloseStreams 关闭一个流

  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img11.png)
- /MediaServer/GetStreamList 获取流列表
  
  参数：流媒体ID  获取到MediaServer中存在的所有流，并显示信息
- /MediaServer/OpenRtpPort  打开某个rtp端口
   ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img12.png)
   
  打开端口成功后，要立即往 这个端口推流，否则将会超时回收。
- /MediaServer/CloseRtpPort 关闭某个rtp端口
   ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img13.png)
   
  关闭流媒体服务器上stream_Id为xxxx的流所在推的端口
- /MediaServer/GetRtpPortList 获取流媒体已经开放的rtp端口列表
   
  参数：流媒体服务器ID 
- /System/GetGlobleSystemInfo 获取全局的系统信息

  参数：无
### 摄像头
- /MediaServer/AddCameraInstance 注册添加一个摄像头实例
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img14.png)
- /MediaServer/DeleteCameraInstance 删除一个摄像头实例
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img15.png)

删除ID为6rt5K4YtkuiNv0FM 的zlm上的摄像头ID为8E1AB912的摄像头
- /MediaServer/ModifyCameraInstance 修改一个注册摄像头实例
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img16.png)
- /MediaServer/GetCameraInstanceList 获取摄像头实例列表
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img17.png)

查询zlmID 为6rt5K4YtkuiNv0FM 的流媒体上的摄像头实例
- /MediaServer/GetCameraInstanceListEx 扩展查询已注册摄像头列表
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img18.png)

查询某个zlm上激活了多少个摄像头。
- /MediaServer/GetCameraSessionList 获取在线摄像头列表
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img19.png)

此接口是查询未激活的摄像头个数
- /MediaServer/GetCameraSessionByCameraId 根据摄像头ID查询在线摄像头对象
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img20.png)
- /MediaServer/GetSipDeviceIdFromCameraId  通过流媒体ID与摄像头实例ID获取SipDeviceId
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img21.png)
### ZLMediaKit录制
- /MediaServer/StartRecord启动流的录制

![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img22.png)

Vhost、app、stream  请查看zlm的wiki 《播放URL规则》篇,链接如下：

https://github.com/xia-chu/ZLMediaKit/wiki/%E6%92%AD%E6%94%BEurl%E8%A7%84%E5%88%99

接口 /MediaServer/StopRecord 停止流的录制与接口 /MediaServer/GetRecordStatus 获取流的录制状态  ，参数与录制一样，根据自己需求选择zlm ID即可，在此不述。

### AKStream录制计划
- 先调用接口：  /DvrPlan/CreateDvrPlan 创建录制计划

  参数说明如图：
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img23.png)

  调用成功StreamNode日志：
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img24.png)
- /DvrPlan/GetDvrPlan 获取录制计划

  参数请求:
  
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img25.png)
  
  Responses(回复)：
  
   ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img26.png)
- /DvrPlan/OnOrOffDvrPlanById 启用或停用一个录制计划
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img27.png)
- /DvrPlan/DeleteDvrPlanById  删除一个录制计划ById
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img28.png)
  
  填入录制计划的ID
- /DvrPlan/SetDvrPlanById 修改录制计划ById
  
  与创建录制计划参数一样，选择一个你要修改的录制计划进行修改
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img29.png)
- /MediaServer/GetDvrVideoById 根据id获取视频文件信息
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img30.png)
  
  回复的内容为视频的详细信息，如下图
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img31.png)
- /MediaServer/GetDvrVideoList 获取录像文件(条件灵活)
  ![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img32.png)
  
  返回所有查找到的视频信息
- /MediaServer/SoftDeleteDvrVideoById 删除一个录像文件ById（软删除，只做标记，不删除文件，文件在24小时后删除）
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img33.png)
- /MediaServer/UndoSoftDelete 恢复被软删除的录像文件
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img34.png)
- /MediaServer/HardDeleteDvrVideoById删除一个录像文件ById（硬删除，立即删除文件，数据库做delete标记）
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img35.png)
- /MediaServer/HardDeleteDvrVideoByIdList 删除一批录像文件ById（硬删除，立即删除文件，数据库做delete标记）
![image](https://github.com/linshangqiang/AKStream-Readme/blob/main/img36.png)
### PTZ控制：
等待更新
### 裁剪
等待更新
