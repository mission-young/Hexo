title: macOS配置Qt开发环境
author: 远方
tags:
  - macOS
  - qt
categories: []
date: 2019-10-10 18:14:00
---
## 安装qt和qtcreator
```bash
brew install qt
brew cask install qt-creator
```
## 配置qt环境
```bash
echo 'export PATH="/usr/local/opt/qt/bin:$PATH"' >> ~/.zshrc
echo 'export LDFLAGS="-L/usr/local/opt/qt/lib"' >> ~/.zshrc
echo 'export CPPFLAGS="-I/usr/local/opt/qt/include"' >> ~/.zshrc
```
