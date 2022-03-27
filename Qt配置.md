# Qt配置及问题解决

## Qt配置

### 1. 从清华镜像源下载安装包

`https://mirrors.tuna.tsinghua.edu.cn/qt/official_releases/online_installers/qt-unified-linux-x64-online.run`
设置代理为无代理，直接从清华镜像下载

### 2. 安装

安装路径改为`opt/Qt`

### 3. 配置

1. 修改`/usr/lib/x86_64-linux-gnu/qt-default/qtchooser/default.conf`中的路径为Qt的路径，如：

```
/opt/Qt/6.2.4/gcc_64/bin
/opt/Qt/6.2.4/gcc_64/lib
```

2. 在`etc/profile`里添加 
```
export QTDIR=/opt/Qt/Tools/QtCreator
export PATH=$QTDIR/bin:$PATH
export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PATH
export Qt6_DIR=/opt/Qt/6.2.4/gcc_64 #注意这句放在第二行会导致QtCreator找不到QString等
```

如果项目的`CMakeLists.txt`没写好，可能会出现`找不到QString文件`等错误，见[Cmake找不到Qt6相关内容](#cmake找不到qt6相关内容)


## 相关错误解决

### Qt Creator无法使用中文

见[Qt Creator无法使用中文](Qt%20Creator无法使用中文.md)

### Cmake找不到Qt6相关内容

#### 1. 背景：`CMakeLists.txt`的内容：
```C++
cmake_minimum_required(VERSION 3.18)

set(PROJECT_NAME memory)
project(${PROJECT_NAME})

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

find_package(Qt6 COMPONENTS Core REQUIRED)

set(HEADERS
    memory_info.h
)

set(SOURCES
    main.cpp
    memory_info.cpp
)

add_executable(${PROJECT_NAME}
    ${HEADERS}
    ${SOURCES}
)

target_link_libraries(${PROJECT_NAME} Qt6::Core)

```

#### 2. 错误：`cmake ..`时报错内容
```bash
Make Error at CMakeLists.txt:14 (find_package):
  By not providing "FindQt6.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "Qt6", but
  CMake did not find one.

  Could not find a package configuration file provided by "Qt6" with any of
  the following names:

    Qt6Config.cmake
    qt6-config.cmake

  Add the installation prefix of "Qt6" to CMAKE_PREFIX_PATH or set "Qt6_DIR"
  to a directory containing one of the above files.  If "Qt6" provides a
  separate development package or SDK, be sure it has been installed.
```

#### 3. 解决办法
   1. 在`CMakeLists.txt`里添加命令
   `set(CMAKE_PREFIX_PATH $ENV{Qt6_DIR})`

   2. 在`/etc/profile`里添加
   `export Qt6_DIR=/opt/Qt/6.2.4/gcc_64`

   3. `source /etc/profile`

### qDebug()不输出内容

#### 解决办法

  编辑`/etc/X11/Xsession.d/00deepin-dde-env`文件，注释掉`export QT_LOGGING_RULES="*.debug=false"`，重启。