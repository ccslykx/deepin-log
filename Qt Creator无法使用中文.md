
# [已解决] Qt Creator无法使用中文

参考https://huguoyang.cn/2022/02/08/解决Linux下qtcreator无法输入中文的问题

## 原因

Ubuntu下QtCreator默认只支持了ibus,所以新安装fcitx5或者fcitx无法在qtcreator中输入中文。
需要自己手动编译fcitx5官方的qt插件

官方仓库：https://github.com/fcitx/fcitx-qt5

### 注意事项
github上还有一个仓库叫fcitx5-qt,千万不要编译这个，两个仓库差不多，我也不知道区别，但是实测这个仓库编译之后不能解决问题。

## 解决记录

### 克隆仓库

```bash
git clone https://github.com/fcitx/fcitx-qt5
cd fcitx-qt
```

### 修改CMakeLists.txt

如果是qt5编译的，则在`CMakeLists.txt`中找到

`option(ENABLE_QT5 "Enable Qt5" Off)`改为`option(ENABLE_QT5 "Enable Qt5" On)`

如果是qt6编译的，则在`CMakeLists.txt`中找到

`option(ENABLE_QT5 "Enable Qt6 im module" Off)`改为`option(ENABLE_QT5 "Enable Qt6 im module" On)`

### 编译
```bash
mkdir build
cd build
cmake .. -DCMAKE_PREFIX_PATH=qt安装目录/6.2.3/gcc_64 -DENABLE_LIBRARY=false
make -j8
```

#### cmake时报错
```bash
Could not find a package configuration file provided by "ECM" (requested
version 1.4.0) with any of the following names:

    ECMConfig.cmake
    ecm-config.cmake

Add the installation prefix of "ECM" to CMAKE_PREFIX_PATH or set "ECM_DIR"
to a directory containing one of the above files. If "ECM" provides a
separate development package or SDK, be sure it has been installed.
```

#### 解决：安装下面的包
```bash
sudo apt install extra-cmake-modules
```

#### 又出错
```bash
dpkg: 处理归档 /var/cache/apt/archives/libqtcore4_4%3a4.8.7.1+dfsg-1+dde_amd64.deb (--unpack)时出错：
 正试图覆盖 /usr/lib/x86_64-linux-gnu/qt-default/qtchooser/default.conf，它同时被包含于软件包 libqt5core5a:amd64 5.15.2+dfsg-13
在处理时有错误发生：
 /var/cache/apt/archives/libqtcore4_4%3a4.8.7.1+dfsg-1+dde_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

#### 强制安装
```bash
sudo dpkg -i --force-overwrite /var/cache/apt/archives/libqtcore4*.deb
```

#### 报错
```bash
dpkg: 依赖关系问题使得 libqtcore4:amd64 的配置工作不能继续：
 libqtcore4:amd64 依赖于 qtcore4-l10n；然而：
  软件包 qtcore4-l10n 尚未配置。

dpkg: 处理软件包 libqtcore4:amd64 (--install)时出错：
 依赖关系问题 - 仍未被配置
dpkg: 依赖关系问题使得 libqtcore4:i386 的配置工作不能继续：
 libqtcore4:i386 依赖于 qtcore4-l10n；然而：
  软件包 qtcore4-l10n 尚未配置。

dpkg: 处理软件包 libqtcore4:i386 (--install)时出错：
 依赖关系问题 - 仍未被配置
正在处理用于 libc-bin (2.31-13+deb11u2) 的触发器 ...
在处理时有错误发生：
 libqtcore4:amd64
 libqtcore4:i386
 ```

#### 解决
```
sudo apt install qtcore4-l10n
```

之后继续上面未完成的操作即可。

### 编译后

编译完成之后，可以在`build`目录下找到`qt6/platforminputcontext/libfcitxplatforminputcontextplugin-qt6.so`

这就是我们要的文件

把他复制到这两个地方：

`qt安装目录/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts`(qtcreator就可以输入中文了)
`qt安装目录/6.2.3/gcc_64/plugins/platforminputcontexts`(你用qt6编译的程序中就可以输入中文了)