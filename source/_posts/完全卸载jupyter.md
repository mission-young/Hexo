title: 完全卸载jupyter
author: 远方
tags:
  - python3
categories: []
date: 2021-03-08 18:04:00
---
使用`pip(3) uninstall jupyter`的方式无法卸载完全，使用`pip-autoremove`包也如此，因而只能采用以下方式：
```bash
pip3 uninstall -y jupyter
pip3 uninstall -y jupyter_core
pip3 uninstall -y jupyter-client
pip3 uninstall -y jupyter-console
pip3 uninstall -y notebook
pip3 uninstall -y qtconsole
pip3 uninstall -y nbconvert
pip3 uninstall -y nbformat
```