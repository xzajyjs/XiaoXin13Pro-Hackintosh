# XiaoXin-13-Pro-Hackintosh  
关键词：`Hackintosh` `XiaoXin` `EFI` `Tourtil`  
## 目录
* [个人配置](#个人配置)  
* [准备工具](#准备工具) 
* [操作流程](#操作流程)  
* 参考资料  
* 问题说明    
## 个人配置  
|规格|详细信息|
|:-|:-|
|型号|联想小新13Pro2020|
|CPU|Intel i5-10210U|
|显卡|~~MX350~~ (独显无法驱动)UHD620|
|内存|板载16g|
|网卡|~~intel AX201~~ ~~DW1820A~~ DW1560|
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

    以下演示使用macOS BigSur 11.0.1 20B50 正式版 with Clover 5126原版镜像[双EFI版][UEFI and MBR]，完全安装后可完美升级至11.2.3。其他镜像版本安装类似。  
## 操作流程  
