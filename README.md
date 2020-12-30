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
摄像头接入成功之后，需要在streamCtrl上激活之后才能推流，需要在调用StreamCtrl接口文档激活摄像头，swagger文档找**到/MediaServer/ActivateSipCamera  激活sip网关自动添加的摄像头这个接口

