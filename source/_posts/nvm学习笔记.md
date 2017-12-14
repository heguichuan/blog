---
title: nvm学习笔记
---
# nodejs版本管理工具——nvm学习笔记

## windows版本

*_下载安装：_*[地址](https://github.com/coreybutler/nvm-windows)，windows下直接安装。

*_使用：_*

```bash
#常用命令
nvm list #列出已安装版本
nvm list available #列出可安装版本
nvm install <version> #安装指定版本号
nvm uninstall <version> #安装指定版本号
nvm use <version> #使用指定版本号
node -v #查看当前node版本号
```

```bash
#帮助
E:\360Downloads\nvm-setup>nvm

Running version 1.1.5.

Usage:

  nvm arch                     : Show if node is running in 32 or 64 bit mode.
  nvm install <version> [arch] : The version can be a node.js version or "latest
" for the latest stable version.
                                 Optionally specify whether to install the 32 or
 64 bit version (defaults to system arch).
                                 Set [arch] to "all" to install 32 AND 64 bit ve
rsions.
                                 Add --insecure to the end of this command to by
pass SSL validation of the remote download server.
  nvm list [available]         : List the node.js installations. Type "available
" at the end to see what can be installed. Aliased as ls.
  nvm on                       : Enable node.js version management.
  nvm off                      : Disable node.js version management.
  nvm proxy [url]              : Set a proxy to use for downloads. Leave [url] b
lank to see the current proxy.
                                 Set [url] to "none" to remove the proxy.
  nvm node_mirror [url]        : Set the node mirror. Defaults to https://nodejs
.org/dist/. Leave [url] blank to use default url.
  nvm npm_mirror [url]         : Set the npm mirror. Defaults to https://github.
com/npm/npm/archive/. Leave [url] blank to default url.
  nvm uninstall <version>      : The version must be a specific version.
  nvm use [version] [arch]     : Switch to use the specified version. Optionally
 specify 32/64bit architecture.
                                 nvm use <arch> will continue using the selected
 version, but switch to 32/64 bit mode.
  nvm root [path]              : Set the directory where nvm should store differ
ent versions of node.js.
                                 If <path> is not set, the current root will be
displayed.
  nvm version                  : Displays the current running version of nvm for
 Windows. Aliased as v.
```