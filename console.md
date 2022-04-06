
## pkg-config

### 查找lib
如：查找libcamera
```shell
ccslykx@deepin:~$ pkg-config --libs --cflags libcamera
-I/usr/local/include/libcamera -L/usr/local/lib/x86_64-linux-gnu -lcamera -lcamera-base
ccslykx@deepin:~$ 
```
在`CMakeLists.txt`里的`target_link_libraries`里需要写`camera`和`camera-base`，而非`libcamera`。