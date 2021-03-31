# Lenovo XiaoXin-13-Pro-Hackintosh  
关键词：`Hackintosh` `XiaoXin` `EFI` `Tutorial` `Lenovo`
## 目录
* [个人配置](#个人配置)  
* [准备工具](#准备工具) 
* [操作流程](#操作流程)  
	* 准备工作
	* 安装系统
	* 完善设置
* [参考资料](#参考资料)  
* [问题说明](#问题说明)    
## 个人配置  
|规格|详细信息|
|:-|:-|
|型号|联想小新13Pro2020|
|CPU|Intel i5-10210U|
|显卡|~~MX350~~ (独显无法驱动)UHD620|
|内存|板载16g（2666Mhz）|
|网卡|BCM94352Z(DW1560)|
|硬盘|西数SN730(1T)|  
    注:1.硬盘512G自行更换为同型号1T  
       2.原装intel AX201无法使用隔空投送、蓝牙，随航，强烈建议更换为DW1560（EFI已完美适配）
       3.DW1820A性价比比DW1560高但有较大可能需要屏蔽针脚且容易掉驱动（并且第二次连接同一热点大概率失败），除非经济实在拮据，否则非常不推荐！  
       4.DW1560可实现功能：随航、接力、隔空投送、Apple Watch解锁
       5.核显在Mac下伪装成UHD630 Mobile  
## 准备工具  
1.配置好的EFI(适合此机型的)  
2.BalenaEtcher-Setup  
3.DiskGenius  
4.EasyUEFI  
5.InsydeH2OUVE  
6.MacOS镜像文件（[下载](https://blog.daliansky.net/categories/%E4%B8%8B%E8%BD%BD/)）  

    以下演示使用macOS BigSur 11.0.1 20B50 正式版 with Clover 5126原版镜像[双EFI版][UEFI and MBR]，完全安装后可完美升级至11.2.3,其他镜像版本安装类似。  
## 操作流程
### 1.准备工作
* 关闭bios中的安全启动，保存并退出。

		小新13Pro进入Bios方式：开机Fn+f2
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/secure%20boot.png?raw=true "关闭Secure Boot")  
* 关闭驱动器Bitlocker（如打开的话）。
* 解锁DMVT和ctglock(**至关重要**)  

		管理员身份运行InsydeH2OUVE文件夹内H2OUVE-W-GUIx64.exe, File---load runtime, 修改如下位置的参数并保存重启:
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/bios_1.png?raw=true "鼠标指针处修改为02")
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/bios_2.png?raw=true)
* 使用BalenaEtcher刻录MacOS镜像至U盘
* 将U盘OC文件夹中的EFI文件夹删除并进行替换（内包含boot和oc两个文件夹）
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/disk.png?raw=true "删除原有的EFI文件夹并完成替换")
* ~~建立Mac分区并更改分区参数为APFS~~ (可有可无)
* 将软件安装包放入dates分区

		其中的OCC需要用到以来配置config.list和进行其他设置
### 2.安装系统
* 重启，进入bios设置EFI USB Device 1为第一启动项

		其中Device为Clover引导，Device1为OC引导，Device2为PE
 * 选择Install macos

		第二次起则选择macOS Install
* 选择磁盘工具--抹掉--APFS格式 

		名称任意，建议小白按照图中的来
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/install.png?raw=true "抹掉磁盘并定义为APFS格式")
* 安装macOS
### 3.完善设置
* 安装DATES中的软件
* 使用OCC挂载两个分区，并将U盘中EFI文件夹内的OC和Boot文件夹拷贝至本地mac分区的EFI文件夹内

		完成后本地mac分区EFI文件夹内应有三个文件夹：Microsoft , Boot , OC  
* （**切勿**推出U盘）重启，进入mac
* Terminal中运行如下代码：

		sudo spctl --master-disable
		sudo kextcache -i /	  (此条运行后需稍等片刻)
* 重启，进入PE

		开机Fn+f12，选择Device2
		此步为修复windows启动项
* 打开UEFI引导修复，选择ESP分区，系统根目录选择C:\\windows\\,修复并重启进入windows

		ESP分区号在DiskGeniues中查看，如ESP分区号为数字，则先在引导修复中挂载
* easyUEFI中如图设置:

		目标分区选择Disk0中的最左边一个分区，描述即为开机时启动项的名称
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/easyUEFI.png?raw=true)
* 调整启动项顺序

		建议将mac放到首启动项，开机默认进入mac。因为使用OC引导windows会出现一些小问题。因此要进入windows时开机Fn+f12选择win进入为纯净。
## 参考资料
* 黑果小兵的部落阁：https://blog.daliansky.net/
* 小新13Pro适用EFI：https://github.com/daliansky/XiaoXinPro-13-hackintosh/releases  
	注意：有些EFI不带有1820A的网卡驱动，下载时请注意！添加1820A网卡驱动方式参考[此处](https://blog.daliansky.net/DW1820A_BCM94350ZAE-driver-inserts-the-correct-posture.html),1560参考[此处](https://blog.daliansky.net/Broadcom-BCM94352z-DW1560-drive-new-posture.html)。

	  提供的EFI中已包含1820A和1560通用网卡驱动。此处纠正一个误区，1820A和1560并非免驱，而是现在部分EFI中（小新的绝大多数）已经包含了网卡和蓝牙驱动。
* B站视频：https://www.bilibili.com/video/BV1ca4y1j7bN
## 问题说明
1.该EFI的OC版本（0.63）目前无法解决内置MIC无法使用的问题。  
2.触摸板偶尔失灵或触摸反馈间断。  
3.DW1820A对于连接iPhone热点存在同一设备名称仅能连接一次的bug。  
4.强烈建议更改三码并关闭“查找此Mac”。  
5.若无法登陆AppleID和AppStore，尝试第四步后更改DNS为114.114.114.114和8.8.8.8。  
6.DW1820A的4.1Ghz蓝牙和2.4Ghz无线局域网干扰严重。  
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/battery.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/bluetooth.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/general.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/gpu.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/ssd.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/wifi.png)
