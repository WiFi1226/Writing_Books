---
date: 1st July 2024
Doctype: Note
Category: 02_Writing Books/Untitled.md
Status: 
abstract: 
Keywords: 
tags:
  - "1"
  - "2"
  - "3"
Version: 1.0.0
License: MIT
Copyright: © 2024 FAN WANG. All rights reserved.
---
Python 环境构建
调用路径，在环境变量里更改：
- 系统
- pyenv（管理版本（基础包），pyenv 被调用时系统原生 python 会被阻止, 因此初次安装后必须显式设定版本）
	- pyenv global 
		- venv 与 deactivate  全局虚拟环境 (额外包)
	- pyenv local
		- - venv 与 deactivate  本地虚拟环境 (额外包)

os 系统
文件路径：绝对路径=工作路径（CWD）+相对路径
文件 A 访问文件 B 与文件 A 的文件夹位置无关