---
title: 记一次写npm命令行包的记录
tags: npm
---

# 记一次写 npm 命令行包的记录

## 步骤

1. 项目目录结构随意，并没有特殊要求，执行 npm publish 即可推送
2. 要实现命令行运行需要在 package.json 中填写 bin 字段，值为指向要执行的 js 文件的地址，**且该文件一定要在顶部加如下代码：`#!/usr/bin/env node`**，bin 的作用为 install 的时候在`node_modules/.bin`目录下添加链接，而该代码的作用为在`.bin`目录中链接文件中指定文件的执行环境，参考如下：

```bash
#!/bin/sh
basedir=$(dirname "$(echo "$0" | sed -e 's,\\,/,g')")

case `uname` in
*CYGWIN*) basedir=`cygpath -w "$basedir"`;;
esac

if [ -x "$basedir/node" ]; then
"$basedir/node"  "$basedir/../test-mock/index.js" "$@"
ret=$?
else
node  "$basedir/../test-mock/index.js" "$@"
ret=$?
fi
exit $ret
```

cmd 文件

```bash
@IF EXIST "%~dp0\node.exe" (
  "%~dp0\node.exe"  "%~dp0\..\test-mock\index.js" %*
) ELSE (
  @SETLOCAL
  @SET PATHEXT=%PATHEXT:;.JS;=;%
  node  "%~dp0\..\test-mock\index.js" %*
)
```

## 踩坑

1. `bin`文件头部一定要加环境变量`#!/usr/bin/env node`说明！
2. 用做传承的文件路径一定要用`path.resolve`转换成绝对路径！
3. 加 --cwd 参数是为了指定 nodemon 的工作目录，默认监听启动入口文件的目录
