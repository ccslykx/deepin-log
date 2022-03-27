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
- dtk

```
sudo apt install git g++ libdtkcore-dev libdtkwidget-dev libdtkgui-dev qtcreator-template-dtk

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
[配置记录](Qt%E9%85%8D%E7%BD%AE.md)

### dde-launcher
[无法启动](https://bbs.deepin.org/zh/post/228632)