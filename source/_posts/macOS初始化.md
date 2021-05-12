title: macOS初始化
author: 远方
tags:
  - macOS
  - 配置
categories:
  - 技术
date: 2019-10-10 18:03:00
---
## 安装homebrew
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
## 安装第三方软件权限问题
```bash
sudo spctl --master disable
```
## 配置Git
```bash
git config --global user.name "mission-young"
git config --global user.email "yuanfangsee@pku.edu.cn"
ssh-keygen -t rsa -C "yuanfangsee@pku.edu.cn"
cat ~/.ssh/id_rsa.pub
## paste it to github
```
## 配置开发环境
- [Hexo博客](https://mission-young.github.io/2019/08/18/多端编辑Hexo博客/)
- [Qt]()
- root
```bash
brew install root
```
- tmux 
```bash
brew install tmux
```
``` .tmux.conf
# Send prefix
set-option -g prefix C-a
unbind-key C-a
bind-key C-a send-prefix
# Use Alt-arrow keys to switch panes
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
# Shift arrow to switch windows
bind -n S-Left previous-window
bind -n S-Right next-window
# Mouse mode
set -g mouse on

# Set easier window split keys
bind-key v split-window -h
bind-key h split-window -v
# Easy config reload
bind-key r source-file ~/.tmux.conf \; display-message "tmux.conf reloaded"
```
- [geant4]
- ssh 
```bash
echo 'alias sshribll="ssh -Y -p 2727 wuchenguang@162.105.151.64"' >> ~/.zshrc
```
- xquartz
```bash
brew cask install xquartz
```