# Deepin 安装记录

## 安装软件

### 手动安装软件
- vscode
- Qv2ray
- v2ray-core
- Qt

### 应用商店安装
- WPS 2019
- Telegram
- uTools

### 命令安装
- git
- g++ 
- cmake
- dtk
- net.qv2ray.qv2ray 
- meson

```
sudo apt install git g++ cmake libdtkcore-dev libdtkwidget-dev libdtkgui-dev qtcreator-template-dtk net.qv2ray.qv2ray meson

```

## 配置

### git
```bash
git config --global user.name ${name}
git config --global user.email ${email}
git config --global https.proxy https://127.0.0.1:${port}
git config --global http.proxy http://127.0.0.1:${port}
ssh-keygen -t rsa -C ${email@email.com} // 手动在github里添加public key
```

### Qt
[Qt配置记录](Qt%E9%85%8D%E7%BD%AE.md)

### dde-launcher
[问题：无法启动](https://bbs.deepin.org/zh/post/228632)

### AppImage软件伪装系统安装的软件
以qv2ray为例

0. 解压`AppImage`文件
    ```bash
    *.AppImage --appimage-extract
    ```
   得到`squashfs-root`文件夹，复制`squashfs-root/usr/share/icons/hicolor`到`/usr/share/icons/`
    ``` bash
    sudo cp squashfs-root/usr/share/icons/hicolor /usr/share/icons/
    ```
   打开`squashfs-root/usr/share/icons/hicolor/任意文件夹/apps`，里面有个图片，记录图片文件名，备用。
1. 打开`/opt/apps/`文件夹，新建文件夹`com.qv2ray`
2. 内建两个文件夹`entries`和`files`，一个文件`info`，内容如下：
    ```json
    {
    "appid":"com.qv2ray",
    "name":"qv2ray",
    "version":"2.7.0",
    "arch":[
        "amd64"
    ],
    "permissions":{
        "autostart":false,
        "notification":false,
        "trayicon":false,
        "clipboard":false,
        "account":false,
        "bluetooth":false,
        "camera":false,
        "audio_record":false,
        "installed_apps":false
    },
    "support-plugins":[],
    "plugins":[]
    }
    ```
3. 将`*.AppImage`文件复制到`files`文件夹下
4. 在`entries`文件夹内新建文件夹`applications`
5. 在`applications`文件夹新建文件`qv2ray.desktop`（`程序名.desktop`），内容如下：
    ```ini
    [Desktop Entry]
    Version=2.7.0
    Name=qv2ray
    Comment=qv2ray
    Exec=/opt/apps/com.qv2ray/files/Qv2ray-v2.7.0-linux-x64.AppImage -- %u
    Icon=qv2ray
    Terminal=false
    StartupWMClass=qv2ray
    Type=Application
    Categories=Network;InstantMessaging;Qt;
    Keywords=qv2ray;
    X-GNOME-UsesNotifications=true
    X-Deepin-CreatedBy=com.deepin.dde.daemon.Launcher
    X-Deepin-AppID=com.qv2ray
    ```
   其中`Icon`对应的是第`0`步记录的文件名（不含扩展名），重启后生效。
6. (可选) 将`qv2ray.desktop`复制到桌面。

目录结构 (由`tree`命令生成)
```bash
com.qv2ray
├── entries
│   └── applications
│       └── qv2ray.desktop
├── files
│   └── Qv2ray-v2.7.0-linux-x64.AppImage
└── info
```

### 输入法

#### 设置
控制中心-键盘和语言-输入法-输入法管理-五笔拼音-设置（齿轮图标）
设置（中间一个扳手图标）
显示高级选项打勾-自动上屏取消打勾

控制中心-键盘和语言-输入法-高级设置-全局配置-更多快捷键-额外的切换首...改为左Shift

#### 五笔莫名其秒消失了 [解决办法]([Bug]%20fcitx-table-wubi.md)
~~重新安装解决~~
```bash
sudo apt install fcitx-table-wbpy fcitx-table-wubi
```