华擎 Deskmini 110 黑苹果 Monterey
# 硬件
主板: 华擎 Deskmini 110/COM

处理器: Kaby Lake 台式 （i几-7几00）

显卡: HD630

Wi-Fi和蓝牙: DW1560

有线网卡: Intel I219V2 （板载）

NVME硬盘: 三星 SM951 256GB

# C 效果：
## 完美：
- [x] 处理器自动节能降频
- [x] 显卡图形加速
- [x] Wi-Fi
- [x] BT蓝牙
- [x] 有线网卡千兆自适应
- [x] USB口
- [x] DP 4K 显示
- [x] 接力、隔空传送什么的
- [x] iMessage & Facetime
- [x] HEVC硬解
## 木有测：
- [ ] HDMI （接口坏了）
- [ ] 耳麦口
- [ ] DRM回放（我木有接显示器）
- [ ] VGA口（估计不能用）
## 不行：
- [ ] 休眠（其实木有测试，肯定不能用）


# Dm 安装
## U盘

[OpenCore提供的安装教程在这里](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#creating-the-usb).

嫌长不想看的话跟着我做：

首先得有一个macOS电脑，Intel或者M1的都行；还需要一个U盘，至少16G，实际“烧录”的时候比较挑盘，写不进去就再买一个。

### 下载安装文件
去苹果商店下载[Monterey](https://apps.apple.com/cn/app/macos-monterey/id1576738294?mt=12)，目前（2022年1月1日）还是12.1。

或者
```bash
mkdir ~/macOS-installer
cd ~/macOS-installer
curl https://raw.githubusercontent.com/munki/macadmin-scripts/main/installinstallmacos.py > installinstallmacos.py
sudo python ./installinstallmacos.py
```
输入其中的`002-42435       12.1    21C52  2021-12-13  macOS Monterey`前面的序号下载。

注：
可能需要安装`xattr`:
```bash
pip install xattr
```
或者：
```bash
conda install -c conda-forge xattr
```
详见[利用Conda管理Python环境](https://miyunda.com/setup-python/)。

等一小会，就下载好了dmg文件，挂载打开把里面内容拖到`/Applications`。
### 格式化U盘
不会格式化的点这里看[苹果官网教程](https://support.apple.com/zh-cn/guide/disk-utility/dskutl14079/21.0/mac/12.0)

U盘的新名字是`MyVolume`, 格式为Mac OS Extended。
### “烧录”
```bash
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```
详见[苹果官网教程](https://support.apple.com/zh-cn/HT201372)。

### EFI
我们用[MountEFI](https://github.com/corpnewt/MountEFI)来挂载EFI分区，上面烧录的时候做了一个空白的但是默认不挂载：
```bash
git clone https://github.com/corpnewt/MountEFI
cd MountEFI
chmod +x MountEFI.command
./MountEFI.command
```
它会列出所有盘，看清楚U盘是哪一个，按前面的数字就好了，一般是“1”。

### 有码

用[SMBIOS](https://github.com/corpnewt/GenSMBIOS)生成码，示例中生成5次，可以自行挑选。

**不要用我的。**

**不要用我的。**

**不要用我的。**
```bash
git clone https://github.com/corpnewt/GenSMBIOS
cd GenSMBIOS
chmod +x GenSMBIOS.command
./GenSMBIOS.command
```
输入“3”然后输入`Macmini 8,1 5`，挑一组顺眼的存起来。

### U盘制作完成

下载[EFI-deskmini110-KBL-dw1560-Monterey.zip](https://github.com/miyunda/deskmini110-monterey/releases/download/v1.0.0/EFI-deskmini110-KBL-dw1560-Monterey.zip)。

解压缩然后将 `EFI-deskmini110-KBL-dw1560-Monterey` 改名为 `EFI`，用文本编辑器打开`EFI/OC/config.plist` 将里面的`SystemProductName`, `SystemSerialNumber`, `MLB` 还有 `SystemUUID`的值换成自己的码，然后保存。
把**整个**`EFI`文件夹复制到U盘的EFI分区。

## Deskmini 110
### BIOS
先改BIOS设置

禁用：
- [ ] Fast boot
- [ ] Secure boot
- [ ] CSM
- [ ] Intel SGX
- [ ] Intel Platform Trust
- [ ] Serial port

启用:
- [x] VT-D
- [x] HT
- [x] XHCI Hand-off
- [x] DVMT Pre-Allocated(iGPU Memory): 64MB
### 终于能安装了
此段木有可说的，注意别同意各种发送数据给苹果参加调查。

### 安装好之后
去新电脑上把U盘和内置硬盘的EFI分区都挂载好，把**整个**`EFI`文件夹从U盘EFI分区复制到内置硬盘的EFI分区。

都弄好了别忘记去时间机器备份下。
感谢以下大佬无私奉献：
[@dortania](https://github.com/dortania)
[@corpnewt](https://github.com/corpnewt)
[@acidanthera](https://github.com/acidanthera)
[@xRetia](https://github.com/dfc643)
