# make

## 将make的结果输出到文件

输出内容分为两种：
- 正确内容: `1>`
- 编译错误信息: `2>`
在bash中:
```bash
make > right.log      # 只将编译正确的信息输出到right.log文件中
make 2> error.log     #只将编译错误的信息输出到error.log中
make > build.log 2>&1 # 将正确和错误的Log全都输出到build.log文件中
```