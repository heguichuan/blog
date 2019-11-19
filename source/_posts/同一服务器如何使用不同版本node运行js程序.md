---
title: 同一服务器使用不同nodejs版本分别运行对应程序
tags: nodejs
---

# 同一服务器使用不同nodejs版本分别运行对应程序

## node8以后npm自带的功能

[科普文：运维不给升级 Node 版本怎么办](https://www.yuque.com/egg/nodejs/node-multi-versioning)

```bash
npm i -S node@lts #安装node包
npx node@10 index.js #指定node版本运行

# 如果低版本的npm不支持npx，则可以手动安装npx包到本地
# npm i -S npx
# 再在package.json中加入如下命令："start": "./node_modules/.bin/npx node@10 index.js"
```

## 使用nvm自带功能

```bash
nvm run 10.16.3 index.js #nvm指定node版本运行脚本，nvm -h 查看帮助文档
```

## 使用pm2指定执行程序路径

[pm2能指定node版本运行吗？](https://cnodejs.org/topic/597fecfe8f0313ff0d08d9d9)

[pm2文档](http://pm2.keymetrics.io/docs/usage/application-declaration/#attributes-available)

```bash
pm2 start index.js --interpreter /root/.nvm/versions/node/v10.16.3/bin/node -n appName #interpreter参数指定node路径
```
