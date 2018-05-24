---
title: loading-加载动画总结
tags: css,前端,笔记
notebook: 笔记&博客
---
# loading - 加载动画总结

## onenet篇

### 1、表格内容加载动画

**html:**

```javascript
loadingStr(colspan){
        return '<tr style="width: 100%;text-align: center;"><td colspan="'+colspan+'">'
        + '<div class="load-wrapper loading-wrapper"><i class="icon-spin5 animate-spin"></i></div>'
        + '</td></tr>';
},
```

**css:**

引入common.less就可以。

### 2、页面加载动画

**html:**

```javascript
_loadingDomStr () {
    return $(['<div class="show-loading">',
              '<div class="loading">',
              '<div class="spin">',
              '<div class="load">',
              '<span></span><span></span><span></span>',
              '</div>',
              '</div>',
              '</div>',
              '</div>'].join(' '));
},
```

**css:**

```less
@import url("/common/css/module/multislider.less");
.show-loading {
    width: 100%;
    height: 100px;
	text-align: center;
	line-height: 100px;
	.loading {
		position: absolute;
		top: 50%; left: 50%;
		transform: translate(-50%, -50%);
	}
}
```

## 通用篇

> 主要是有三个开源库：
>
> 1. [SpinKit](https://github.com/tobiasahlin/SpinKit) 可以直接拷贝demo代码，用哪个拷哪个很方便。
> 2. [loaders.css](https://github.com/ConnorAtherton/loaders.css) 另外一个实现，有react、vue等版本。
> 3. [loading](https://github.com/jxnblk/loading/) svg动画。
