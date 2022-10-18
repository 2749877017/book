# js实现纯前端读取excel文件

## 一、下载SheetJS文件

### 1.在本机或者u盘中可以找到，也可以去官网[sheetjs](https://github.com/SheetJS/sheetjs)进行下载

下载之后打开其中的dist文件目录，能看到其中有xlsx.core.min.js和xlsx.full.min.js两个JS文件，使用其中一个文件就行，一般情况下使用xlsx.core.min.js就可以了

## 二、用法

### 1.首先在HTML中定义如下

```html
<input type="file" id="inputfilename">    <!--选择文件的input-->

<button onclick="readWorkbookFromLocalFile()">读取Excel表格</button> <!--读取文件的按钮-->

<div id="result"></div> <!--显示所读取excel的区域-->
```

### 2. 引入JS

```html
<script src="../../js/xlsx.core.min.js"></script>
```

### 3. 使用XLSX.read方法读取本地excel文件

```js
 function readWorkbookFromLocalFile() {
        var file = document.getElementById('inputfilename').files[0];
        if (file) {
          var reader = new FileReader();

          reader.onload = function (e) {
            var data = e.target.result;
            var workbook = XLSX.read(data, { //XLSX.read()方法会返回一个workbook 对象
              type: 'binary'
            });
            readWorkbook(workbook);
          };
          reader.readAsBinaryString(file);

        } else {
          alert('请先选择文件');
        }
      }
```

### 4. 得到exce文件的csv和json格式

得到csv格式是为了在网页中显示读取到的表格  
得到json格式是为了与后台进行数据交互

```js
var json = null;
var csv = '';

 function readWorkbook(workbook) {

   var sheetNames = workbook.SheetNames; // 工作表名称集合
   var worksheet = workbook.Sheets[sheetNames[0]]; // 这里我们只读取第一张sheet的内容

   csv = XLSX.utils.sheet_to_csv(worksheet); //这里便可以得到csv格式
   document.getElementById('result').innerHTML = csv2table(csv);  //使用csv2table()函数将其转换为简单的HTML表格，csv2table()函数定义在下一个代码块中

   json = XLSX.utils.sheet_to_json(worksheet); 
   // 成功转换为json格式后，可能表格中的中文属性名并不是后台所需要的字段名
   // 那么，就可以使用如下方法，遍历这个json对象，然后对其中的字段名进行修改
   for (var i in json) {
     for (var key in json[i]) {
       if (key == '年龄') {
         json[i]['age'] = json[i][key] //修改属性名为“age”
         delete json[i]['年龄'] //删除“年龄”
         } else if (key == '性别') {
         if (json[i][key] == '男') {
           json[i][key] = '1';
         } else {
           json[i][key] = '0';
         }
         json[i]['sex'] = json[i][key] //修改属性名为“sex”
         delete json[i]['性别'] //删除“性别”
       } else if (key == '姓名') {
         json[i]['username'] = json[i][key] //修改属性名为“username”
         delete json[i]['姓名'] //删除“姓名”
       } else if (key == '工号') {
         json[i]['workId'] = json[i][key] + ''; //修改属性名为“workId”
         delete json[i]['工号'] //删除“工号”
       }
     }
   }
 }
```

到这里就能够实现通过SheetJS在页面中显示从本地读取到的excel文件了，也能够拿到想要与后台进行交互时的标准json数据格式，最后只需要发送Ajax与后台进行交互就OK拉！
