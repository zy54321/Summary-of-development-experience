# JS生成并下载txt文件

## 实现

下面的简单函数允许您直接在浏览器中生成文件，而无需接触任何服务器。它适用于所有HTML5就绪的浏览器，因为它使用了<a>的下载属性：

```javascript
function download(filename, text) {
  var element = document.createElement('a');
  element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(text));
  element.setAttribute('download', filename);
 
  element.style.display = 'none';
  document.body.appendChild(element);
 
  element.click();
 
  document.body.removeChild(element);
}
 
 
download("hello.txt","This is the content of my file :)");
```

创建库，[FileSaver.js](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Feligrey%2FFileSaver.js)在不支持`saveAs()`的FileSaver接口的浏览器中实现它。如果您需要保存更大的文件，或者BLOB的大小限制，或者没有足够的内存，那么请看一看更高级的[StreamSaver.js](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fjimmywarting%2FStreamSaver.js)，它可以使用新的StreamsAPI的强大功能将数据直接异步保存到硬盘中。同时支持进度查看，取消和何时完成。
下面的代码段允许您生成一个文件(具有任何扩展名)并下载它，而无需链接任何服务器：

```javascript
var content = "What's up , hello world";
// any kind of extension (.txt,.cpp,.cs,.bat)
var filename = "hello.txt";
 
var blob = new Blob([content], {
 type: "text/plain;charset=utf-8"
});
 
saveAs(blob, filename);
```

## 下表显示了FileSaver.js在不同浏览器中的兼容性：

| Browser            | Constructs as | Filenames | Max Blob Size | Dependencies                                                 |
| ------------------ | ------------- | --------- | ------------- | ------------------------------------------------------------ |
| Firefox 20+        | Blob          | Yes       | 800 MiB       | None                                                         |
| Firefox < 20       | data: URI     | No        | n/a           | [Blob.js](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Feligrey%2FBlob.js) |
| Chrome             | Blob          | Yes       | [500 MiB][3]  | None                                                         |
| Chrome for Android | Blob          | Yes       | [500 MiB][3]  | None                                                         |
| Edge               | Blob          | Yes       | ?             | None                                                         |
| IE 10+             | Blob          | Yes       | 600 MiB       | None                                                         |
| Opera 15+          | Blob          | Yes       | 500 MiB       | None                                                         |
| Opera < 15         | data: URI     | No        | n/a           | [Blob.js](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Feligrey%2FBlob.js) |
| Safari 6.1+*       | Blob          | No        | ?             | None                                                         |
| Safari < 6         | data: URI     | No        | n/a           | [Blob.js](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Feligrey%2FBlob.js) |
| Safari 10.1+       | Blob          | Yes       | n/a           | None                                                         |

