---
title: python打包
date: 2025-01-04T17:16:21+08:00
categories: [Python]
tags: [Python,Utils]
---

## 安装pyinstaller工具

``` bash
pip install pyinstaller==6.2.0
#装别的版本,打包带--noconsole会报错 
#(225, '', '无法成功完成操作，因为文件包含病毒或潜在的垃圾软件。') 
```

## 打包单python文件加配置文件
``` bash 
pyinstaller --onefile --add-data "config.ini;." DbxController.py --noconsole
#--noconsole 打包后不会有终端窗口
```