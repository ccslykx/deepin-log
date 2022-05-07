
## pkg-config

### 查找lib
如：查找libcamera
```shell
ccslykx@deepin:~$ pkg-config --libs --cflags libcamera
-I/usr/local/include/libcamera -L/usr/local/lib/x86_64-linux-gnu -lcamera -lcamera-base
ccslykx@deepin:~$ 
```
在`CMakeLists.txt`里的`target_link_libraries`里需要写`camera`和`camera-base`，而非`libcamera`。


## dpkg

### 安装依赖
当执行完`sudo dpkg -i *.deb`命令后，会出现缺少依赖报错，使用下面命令一键安装全部依赖：
```shell
sudo apt install -f
```

##