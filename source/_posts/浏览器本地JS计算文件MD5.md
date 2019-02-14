---
title: 浏览器本地JS计算文件MD5
date: 2019-2-14 13:59:18
---

# 浏览器本地JS计算文件MD5

> [js-spark-md5](https://github.com/satazor/js-spark-md5/tree/3.0.0)

## 全量计算

### 示例

```js
let MD5IsRunning = false;
getFileMD5(file) {
    const errTipText = "MD5计算失败，请重新选择文件！";

    return new Promise((resolve, reject) => {
        if (MD5IsRunning) {
            reject("操作繁忙！");
        }

        if (!file) {
            reject(errTipText);
        }

        let fileReader = new FileReader();

        fileReader.onload = e => {
            MD5IsRunning = false;
            if (file.size != e.target.result.byteLength) {
                reject(errTipText);
            } else {
                resolve(SparkMD5.ArrayBuffer.hash(e.target.result));
            }
        };

        fileReader.onerror = function() {
            MD5IsRunning = false;
            reject(errTipText);
        };

        MD5IsRunning = true;

        fileReader.readAsArrayBuffer(file);
    });
}
```



## 增量计算

```js
function doIncrementalTest(file) {
    if (running) {
        return;
    }

    var blobSlice = File.prototype.slice || File.prototype.mozSlice || File.prototype.webkitSlice,
        chunkSize = 2097152, // read in chunks of 2MB
        chunks = Math.ceil(file.size / chunkSize),
        currentChunk = 0,
        spark = new SparkMD5.ArrayBuffer(),
        fileReader = new FileReader();

    fileReader.onload = function (e) {
        spark.append(e.target.result); // append array buffer
        currentChunk += 1;

        if (currentChunk < chunks) {
            loadNext();
        } else {
            running = false;
            console.log('success:', spark.end()); // compute hash
        }
    };

    fileReader.onerror = function () {
        running = false;
        console.log('error');
    };

    function loadNext() {
        var start = currentChunk * chunkSize,
            end = start + chunkSize >= file.size ? file.size : start + chunkSize;

        fileReader.readAsArrayBuffer(blobSlice.call(file, start, end));
    }

    running = true;
    loadNext();
}
```