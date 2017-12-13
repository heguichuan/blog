# hexo使用笔记

## 常用命令

```bash
hexo new [layout] <title> 写新文章
hexo g 生成
hexo server 本地运行
hexo deploy 部署
```

## 部署配置

*_git方式_*

安装： `npm install hexo-deployer-git --save`

修改`_config.yml`文件：

```yaml
deploy:
  type: git
  repository: git@github.com:heguichuan/heguichuan.github.io.git
  branch: master
```