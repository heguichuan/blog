# 博客`heguichuan.github.io`源代码仓库

## 使用前准备

1. 安装 npm/yarn
2. 安装 hexo-cli `yarn global add hexo-cli`
3. 安装 git, 部署的时候需要用到

## 使用方法

```bash
git clone
yarn / npm install

# 修改文章，本地预览
hexo server
# or部署
hexo clean
hexo g #生成
hexo d #部署到github.io

# 更新完毕后
git commit
git push
```

## 问题与解决

**1.新装的操作系统部署的时候报错**

```bash
INFO  Copying files from extend dirs...
'git' �����ڲ����ⲿ���Ҳ���ǿ����еĳ���
���������ļ���
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: spawn git ENOENT
    at notFoundError (E:\work\hexo\blog\node_modules\cross-spawn\lib\enoent.js:11:11)
    at verifyENOENT (E:\work\hexo\blog\node_modules\cross-spawn\lib\enoent.js:46:16)
    at ChildProcess.cp.emit (E:\work\hexo\blog\node_modules\cross-spawn\lib\enoent.js:33:19)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:198:12)
```

**解决方法：** 使用 git bash 执行部署命令可解决。
