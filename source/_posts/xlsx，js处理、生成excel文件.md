# xlsx，js处理、生成excel文件.md

## 参考

[导出为excel的例子](http://sheetjs.com/demos/modify.html)
[文档](https://docs.sheetjs.com/#working-with-the-workbook)
[js-xlsx/github](https://github.com/SheetJS/js-xlsx)

## json生成xlsx文件关键代码

```html
<div onclick="doit('xlsx')">abc</div>
```

```javascript
function doit(type) {
    var jsonData = [{
        a:1,
        b:2,
        c:3
    },
    {
        a:11,
        b:22,
        c:33
    },
    {
        a:21,
        b:22,
        c:23
    }]
    var wb = XLSX.utils.book_new(); //生成新的workbook实例
    var ws = XLSX.utils.json_to_sheet(jsonData); //将json转化为worksheet对象
    XLSX.utils.book_append_sheet(wb, ws, '表单11'); //向workbook实例中添加表单(worksheet)
    XLSX.writeFile(wb, 'test.xlsx');  //保存文件
}
```