# Lenovo XiaoXin-13-Pro-Hackintosh
[Chinese Version 中文版本](./ch_README.md)  
KeyWords：`Hackintosh` `XiaoXin` `EFI` `Tutorial` `Lenovo`  
**Warning: Be sure to change the serial number and device code in config.plist after installation (Google search 'three-code modification tutorial'), otherwise the permanent ban of Apple ID has nothing to do with the author!**
## Menu
* [Configuration](#Configuration)  
* [Tools](#Tools) 
* [Operating procedures](#Operating-procedures)  
	* Preprations
	* Install the system
	* Perfect settings
* [Reference](#Reference)  
* [Problem description](#Problems)    
## Configuration
|Specification|Info|
|:-|:-|
|Model|Lenovo Xiaoxin_13Pro2020|
|CPU|Intel i5-10210U|
|Graphics card|~~MX350~~ (cannot be drived)UHD620|
|Memory|16g（2666Mhz）|
|Network card|BCM94352Z(DW1560)|
|Hard disk|WDC-SN730(1T)|  
    FAO:1.Replace 512G hard disk with 1T of the same model.  
       2.The original intel AX201 cannot be used for airdrop, Bluetooth, and onboard. It is strongly recommended to replace it with DW1560 (EFI has a built-in driver for perfect adaptation）  
       3.DW1820A is more cost-effective than DW1560, but it may need to shield the pins and be easy to drive off (and the second connection to the same hotspot is likely to fail). Unless the economy is really tight, it is not recommended!  
       4.DW1560 can realize the functions: Sidecar, Handoff, Airdrop, Apple Watch unlock (unlocking will be invalid in 11.4 and later)
       5.Nuclear display disguised as UHD630 Mobile under Hackintosh.  
       6.After testing, the OC0.6.3 I provided is compatible with any version in 11.0.0-11.5.1 (within the range of the version can be painlessly upgraded), other versions can be tried by yourself!
## Tools
1.Configured EFI (suitable for this model)  
2.BalenaEtcher-Setup（[Download](https://pan.baidu.com/s/11ObMtIKhUBLLy_J3pLN_Pg) Code: me8f）  
3.DiskGenius  
4.EasyUEFI  
5.InsydeH2OUVE  
6.MacOS Mirror file（[Download](https://blog.daliansky.net/categories/%E4%B8%8B%E8%BD%BD/)）  

    The following demonstration uses the official version of macOS BigSur 11.0.1 20B50 with Clover 5126 original image [Dual EFI version][UEFI and MBR], which can be perfectly upgraded to 11.5.1 after complete installation. The installation of other versions is similar.  
## Operating-procedures 
### 1.Preprations
* Close the safe boot in the bios, save and exit.

		Xiaoxin 13Pro enters Bios mode: press Fn+f2 when booting up.
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/secure%20boot.png?raw=true "关闭Secure Boot")  
* Turn off the drive Bitlocker (if turned on).
* Unlock DMVT and ctglock (**critical**)  

		As an administrator, run H2OUVE-W-GUIx64.exe, File---load runtime in the InsydeH2OUVE folder, modify the parameters in the following locations and save and restart:
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/bios_1.png?raw=true "鼠标指针处修改为02")
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/bios_2.png?raw=true)
* Use BalenaEtcher to burn the MacOS ISO to a USB flash drive

        If the format pops up during the burning process, select Cancel.
* Delete and replace the EFI folder in the OC folder of the flash drive (including the boot and oc folders)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/disk.png?raw=true "删除原有的EFI文件夹并完成替换")
* ~~Create a Mac partition and change the partition parameters to APFS~~ (Not neccessary)
* Put the software installation package into the DATAS partition

		The OCC needs to be used to configure config.list and other settings
### 2.Install the system
* Restart, enter bios and set EFI USB Device 1 as the first boot item

		Among them, Device is Clover boot, Device1 is OC boot, Device2 is PE.
 * Choose Install macos

		From the second time choose macOS Install
* Select Disk Utility-Erase-APFS Format 

		The name is arbitrary, it is recommended that Xiaobai follow the picture.
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/install.png?raw=true "抹掉磁盘并定义为APFS格式")
* Install macOS
### 3.Perfect settings
* Install the software in DATES
* Use OCC to mount two partitions, and copy the OC folder in the EFI folder of the flash drive to the EFI folder of the local mac partition, **Attention! Never overwrite the Boot folder in the U disk to the local Boot of the mac! ! ! Otherwise, Windows cannot be booted (boot damage)**

		After completion, there should be three folders in the EFI folder of the local mac partition: Microsoft , Boot , OC  
* (**Don't** eject the USB flash drive) Restart and enter the mac.
* Run the following code in Terminal:

		sudo spctl --master-disable
		sudo kextcache -i /	  (Wait a while after this operation)
* Restart, enter PE

		Press Fn+f12 when booting up, choose Device2.
		Now let's repair windows startup items
* Open UEFI boot repair, select ESP partition, select C:\\windows\\ as the system root directory, repair and restart to enter windows

		Check the ESP partition number in DiskGeniues. If the ESP partition number is a number, mount it in the boot repair first
* Set as shown in easyUEFI:

		Select the leftmost partition in Disk0 as the target partition, and the description is the name of the startup item at boot
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/easyUEFI.png?raw=true)
* Adjust the order of startup items

		It is recommended to put the mac in the first startup item, and enter the mac by default. Because there will be some minor problems when using OC to boot windows. Therefore, when you want to enter windows, turn on Fn+f12 and select win to enter as pure.
## Reference
* 黑果小兵的部落阁：https://blog.daliansky.net/
* DW1820A网卡黑苹果使用教程：https://blog.daliansky.net/DW1820A_BCM94350ZAE-driver-inserts-the-correct-posture.html
* macOS BigSur安装常见问题：https://blog.daliansky.net/Common-problems-and-solutions-in-macOS-BigSur-11.0-installation.html
* 联想小新PRO 13 19/20 Catalina：https://blog.daliansky.net/Lenovo-Xiaoxin-PRO-13-2019-and-macOS-Catalina-Installation-Tutorial.html
* 小新13Pro适用EFI：https://github.com/daliansky/XiaoXinPro-13-hackintosh/releases  
	Note: Some EFI does not have 1820A network card driver, please pay attention when downloading! Add 1820A network card driver method reference [here](https://blog.daliansky.net/DW1820A_BCM94350ZAE-driver-inserts-the-correct-posture.html), 1560 reference[here](https://blog.daliansky.net/Broadcom-BCM94352z-DW1560-drive-new-posture.html).

	  The provided EFI has included 1820A and 1560 universal network card drivers. To correct a misunderstanding here, 1820A and 1560 are not driver-free, but now part of the EFI (the vast majority of Xiaoxin) already contains the network card and Bluetooth driver.
* Video：https://www.bilibili.com/video/BV1ca4y1j7bN
## Problems
1. The OC version (0.63) of this EFI cannot currently solve the problem that the built-in MIC cannot be used.
2. The touchpad occasionally fails or the touch feedback is interrupted.
3. DW1820A has a bug that the same device name can only be connected once to the iPhone hotspot.
4. It is strongly recommended to change the three codes and turn off "Find this Mac".
5. If you cannot log in to AppleID and AppStore, try to change the DNS to 114.114.114.114 and 8.8.8.8 after the fourth step.
6. The 4.1Ghz Bluetooth and 2.4Ghz wireless LAN of DW1820A have serious interference.
7. When DW1560 uses 2.4Ghz Wifi for large bandwidth upload, it will cause Bluetooth to be almost disconnected and unavailable. This is a common problem with dual antenna network cards. Solution: switch to a four-antenna network card or use 5Gh wifi  
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/battery.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/bluetooth.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/general.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/gpu.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/ssd.png)
![](https://github.com/xzajyjs/XiaoXin13Pro-Hackintosh/blob/main/png/wifi.png)
