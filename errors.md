# Errors

## make 

### /usr/bin/ld: 找不到 -lGL

安装`libgl-dev`。

```shell
sudo apt install libgl-dev
```

### /usr/bin/ld: 找不到 -llibcamera

经查找，`libcamera.so`位于`/usr/local/x86_64-linux-gnu`内

尝试：
1. [无效] 在`/etc/profile`里添加`export LD_LIBRARY_PATH=/usr/local/x86_64-linux-gnu:$LD_LIBRARY_PATH`。
2. [无效] 在`/etc/ld.so.conf.d/`下添加[libcamera.conf](/etc/ld.so.conf.d/libcamera.conf)，写入：
   ```
   /usr/local/share/libcamera
   /usr/local/etc/libcamera
   /usr/local/libexec/libcamera
   /usr/local/etc/libcamera/ipa:/usr/local/share/libcamera/ipa
   /usr/local/lib/x86_64-linux-gnu/libcamera
   ```
   注：`libcamera.so`所位于的`/usr/local/x86_64-linux-gnu`路径已经包含在`/etc/ld.so.conf.d/x86_64-linux-gnu.conf`内
3. [无效] 在`/usr/lib`里添加软链接：
   ```shell
   sudo ln -s /usr/local/lib/x86_64-linux-gnu/libcamera libcamera.so
   sudo ln -s /usr/local/lib/x86_64-linux-gnu/libcamera-base.so libcamera-base.so
   ```
4. [解决] 把`CMakeLists.txt`里的`target_link_libraries(${TARGET_NAME} Qt6::Core libcamera)`修改为`target_link_libraries(${TARGET_NAME} Qt6::Core libcamera.so libcamera-base.so)`

### QT undefined reference to `vtable for MyCamera'
[参考](https://blog.csdn.net/u013834525/article/details/93393345)头文件中出现了一些Qt的关键词，如`Q_OBJECT`、`signals`等，这时候要将头文件也放到`CMakeLists.txt`的`add_executable`中，不然不会将这些关键词编译，也就没有办法正确生成类了。如果使用Qt工程，头文件则要放到`.pro`文件的`HEADERS`中。

但是我在`CMakeLists.txt`的`add_executable`里加入了头文件也无效，在删除`Q_OBJECT`宏定义后编译成功

`CMakeLists.txt`里添加`set(CMAKE_AUTOMOC ON)`

### 在Qt工程中，出现`expected unqualified-id before ‘)’ token`错误
`libcamera`中的`slots()`函数名与Qt的宏定义`#define slots Q_SLOTS`冲突。
```log
In file included from /usr/local/include/libcamera/libcamera/camera.h:18,
                 from /usr/local/include/libcamera/libcamera/libcamera.h:11,
                 from /home/ccslykx/Projects/deepin-camera/build/src/deepin-camera_autogen/UVLADIE3JM/../../../../src/src/camera_libcamera.h:23,
                 from /home/ccslykx/Projects/deepin-camera/build/src/deepin-camera_autogen/UVLADIE3JM/moc_camera_libcamera.cpp:10,
                 from /home/ccslykx/Projects/deepin-camera/build/src/deepin-camera_autogen/mocs_compilation.cpp:6:
/usr/local/include/libcamera/libcamera/base/signal.h:31:17: error: expected unqualified-id before ‘)’ token
  SlotList slots();
                 ^
```


## cmake

### Could not find a package configuration file provided by "" with any of the following names:

#### "Qt5LinguistTools"

```shell
sudo apt install qttools5-dev-tools qttools5-dev
```

#### "Qt5Multimedia"
```shell
sudo apt install qtmultimedia5-dev
```

### Could NOT find Boost (missing: filesystem)

```shell
sudo apt-get install libboost-all-dev   # thsi will install all teh required boost components. 
```