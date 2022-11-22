# **Nodejs模块属性**

### 1、引入模块

```js
require('路径') 
const util =  require('./modulse/utils/utils.js')  // 路径
// require() 传参模块路径/模块名称 如果没有写任何路径符 '/' '../' './' require默认模块寻址目录为 root/node_modules文件夹中寻找
// require() 参数直接指定某个文件夹 而非文件的时候 会默认将文件夹内的 index.js 作为模块文件
// require 导入自定义模块名称如果与原生node内置模块冲突 优先使用node内置模块
```

### 2、模块导出

```js
module.exports = { // 可以直接对象的形式导出
	name: 'obj'
}
const obj = {
	name: 'obj'
}
module.exports = obj 	// 也可以直接写exports = obj也能导出，module是默认会加上的
// commonjs 中 js文件模块 不指定exports 导出内容 也会把 module对象的 exports属性默认导出
// 不可以 这样写 exports = {}
```

### 3、module对象和require的关系

```js
require.main: 当前node 程序入口模块的 module对象
console.log(require.main === module, 'app.js') // true 
//如果require.main 和 module相等 就说明这个文件在 当前node程序中为 主入口文件
console.log(require.main.filename) // 获取当前应用程序的入口点

require.main // 入口文件 主程入口  入口模c块(文件) 的 module对象
module // 当前模块(文件) 的模块对象
__dirname  // 当前模块的目录名。 相当于 __filename 的 path.dirname()
console.log(__dirname); // 打印: /Users/mjr
__filename // 当前模块(文件) 的文件名 等同于 module.filename
console.log(__filename); // 打印: /Users/mjr/example.js


```

### 3、module对象

```js
//module对象
{
    id:'模块的标识符。 通常是完全解析后的文件名',
    filename:'模块的完全解析后的文件名',
    loaded:'模块是否已经加载完成，或正在加载中',
    path:'模块的目录名称',
    paths:'模块的搜索路径',
    exports:'导出的模块内容'
}
```



# **Nodejs事件触发器 events**

> events：用于监控用户交互事件

```js
// 引入events，并初始化
const EventEmitter = require('events');
let eventEmit = new EventEmitter();

// on('事件名', () = 回调函数) 定义一个事件
eventEmit.on('start', (参数) => {
  console.log('开始')
})

// emit('事件名', ...参数)  用于触发事件
eventEmit.emit('start')

// once() 参数  同on写法一样 区别: 只触发一次的监听器 触发之后 自动解绑监听
// 比如同时等待两个数据，如果其中一个比较快，那就先触发once()，另外一个就算得到了数据，也不会触发事件了，因为once()只触发一次就解绑，并销毁

// off('事件名必填', function(err){回调函数必传}) 解绑on监听的自定义事件
eventEmit.off('start', function(err){回调函数必传})

提示: node开发中 任何回调异步机制的方法 必须传递回调函数参数 就算你没有回调操作需求 也必须传递callback参数

// removeAllListeners('start事件数组'):移除事件数组的所有监听器。
// 比如给 start 绑定了多个回调函数，那removeAllListeners能一次性全部移除

// eventEmit.eventNames() 返回已on监听的所有事件名称列表，为数组。
```













#  **Nodejs原生模块**

## Buffer(缓冲区)

JavaScript 语言自身只有字符串数据类型，没有二进制数据类型。

但在处理像TCP流或文件流时，必须使用到二进制数据。因此在 Node.js中，定义了一个 Buffer 类，该类用来创建一个专门存放二进制数据的缓存区。

### Buffer 与字符编码

Buffer 实例一般用于表示编码字符的序列，比如 UTF-8 、 UCS2 、 Base64 、或十六进制编码的数据。 通过使用显式的字符编码，就可以在 Buffer 实例与普通的 JavaScript 字符串之间进行相互转换。

> Buffer.from('内容', '转换格式');	// 默认为*ascii*编码

```js
const buf = Buffer.from('runoob', 'ascii');

// 输出 72756e6f6f62	hex格式
console.log(buf.toString('hex'));
// 输出 cnVub29i	base64格式
console.log(buf.toString('base64'));
```

**Node.js 目前支持的字符编码包括：**

- **ascii** - 仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的。
- **utf8** - 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 。
- **utf16le** - 2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。
- **ucs2** - **utf16le** 的别名。
- **base64** - Base64 编码。
- **latin1** - 一种把 **Buffer** 编码成一字节编码的字符串的方式。
- **binary** - **latin1** 的别名。
- **hex** - 将每个字节编码为两个十六进制字符。

------

### 创建 Buffer 类

Buffer 提供了以下 API 来创建 Buffer 类：

- **Buffer.alloc(size, fill, encoding)：** 返回一个指定大小的 Buffer 实例，如果没有设置 fill，则默认填满 0
- **Buffer.allocUnsafe(size)：** 返回一个指定大小的 Buffer 实例，但是它不会被初始化，所以它可能包含敏感的数据
- **Buffer.allocUnsafeSlow(size)**
- **Buffer.from(array)：** 返回一个被 array 的值初始化的新的 Buffer 实例（传入的 array 的元素只能是数字，不然就会自动被 0 覆盖）
- **Buffer.from(arrayBuffer, byteOffset, length)：** 返回一个新建的与给定的 ArrayBuffer 共享同一内存的 Buffer。
- **Buffer.from(buffer)：** 复制传入的 Buffer 实例的数据，并返回一个新的 Buffer 实例
- **Buffer.from(string[, encoding])：** 返回一个被 string 的值初始化的新的 Buffer 实例

```js
// 创建一个长度为 10、且用 0 填充的 Buffer。
const buf1 = Buffer.alloc(10);

// 创建一个长度为 10、且用 0x1 填充的 Buffer。 
const buf2 = Buffer.alloc(10, 1);	// 输出全是1

// 创建一个长度为 10、且未初始化的 Buffer。
// 这个方法比调用 Buffer.alloc() 更快，
// 但返回的 Buffer 实例可能包含旧数据，
// 因此需要使用 fill() 或 write() 重写。
const buf3 = Buffer.allocUnsafe(10);

// 创建一个包含 [0x1, 0x2, 0x3] 的 Buffer。
const buf4 = Buffer.from([1, 2, 3]);

// 创建一个包含 UTF-8 字节 [0x74, 0xc3, 0xa9, 0x73, 0x74] 的 Buffer。
const buf5 = Buffer.from('tést');

// 创建一个包含 Latin-1 字节 [0x74, 0xe9, 0x73, 0x74] 的 Buffer。
const buf6 = Buffer.from('tést', 'latin1');
```

------

### 写入缓冲区

**语法**：写入 Node 缓冲区的语法如下所示： 

```js
buf.write(string, offset, length, encoding])
```

参数描述如下：

- **string** - 写入缓冲区的字符串。
- **offset** - 缓冲区开始写入的索引值，默认为 0 。
- **length** - 写入的字节数，默认为 buffer.length。
- **encoding** - 使用的编码。默认为 'utf8' 。

```js
// 我们填充了一个长度7的Buffer每一位都是 ascii(64)=>(@)
let buf = Buffer.alloc(7, 64);	// ASCII码的64代表是@,默认不写为0
// 往buf中第3位开始写入"hello world!"的前5位值"hello"
let len = buf.write("hello world!", 3, 5)
for (let i = 0; i < buf.length; i++) {
  console.log(buf[i])	// 64 64 64 104 101 108 108
}
// write返回写入buffer的字符长度
console.log(len)	// 4	
// [64,64,64,104,101,108,108] => [@,@,@,h,e,l,l]
console.log(buf.toString('utf-8')); // @@@hell
//[@,@,@,h,e,l,l] 从3读到7 左开右闭
console.log(buf.toString('utf-8', 3, 7), len); //hell 4
```

------

### 从缓冲区读取数据

```js
buf.toString(encoding, start, end)
```

- **encoding** - 使用的编码。默认为 'utf8' 。
- **start** - 指定开始读取的索引位置，默认为 0。
- **end** - 结束位置，默认为缓冲区的末尾。

```js
buf = Buffer.alloc(26);
for (var i = 0 ; i < 26 ; i++) {
  buf[i] = i + 97;
}
console.log( buf.toString('ascii'));       // 输出: abcdefghijklmnopqrstuvwxyz
console.log( buf.toString('ascii',0,5));   //使用 'ascii' 编码, 并输出: abcde
console.log( buf.toString('utf8',0,5));    // 使用 'utf8' 编码, 并输出: abcde
console.log( buf.toString(undefined,0,5)); // 使用默认的 'utf8' 编码, 并输出: abcde
```

------

### 将 Buffer 转换为 JSON 对象

```js
buf.toJSON()
```

当字符串化一个 Buffer 实例时，[JSON.stringify()](https://www.runoob.com/js/javascript-json-stringify.html) 会隐式地调用该 **toJSON()**。

```js
const buf = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5]);
const json = JSON.stringify(buf);
const sonBuf = buf.toJSON();
console.log(json); // 输出: {"type":"Buffer","data":[1,2,3,4,5]}
console.log(sonBuf);	// { type: 'Buffer', data: [ 1, 2, 3, 4, 5 ] }
```

------

### 缓冲区合并

```
Buffer.concat(list[, totalLength])	// 返回一个合并后新的 Buffer 对象。
```

- **list** - 用于合并的 Buffer 对象数组列表。
- **totalLength** - 指定合并后Buffer对象的总长度。

```js
var buffer1 = Buffer.from(('菜鸟教程'));
var buffer2 = Buffer.from(('www.runoob.com'));
var buffer3 = Buffer.concat([buffer1,buffer2]);
console.log("buffer3 内容: " + buffer3.toString());
// 输出： buffer3 内容: 菜鸟教程www.runoob.com
```

------

### 缓冲区比较

Node Buffer 比较的函数语法如下所示, 该方法在 Node.js v0.12.2 版本引入：

```
buf.compare(otherBuffer);	// 0 代表 两个buffer完全一致
```

- **otherBuffer** - 与 **buf** 对象比较的另外一个 Buffer 对象。

**返回值**：返回一个数字，表示 **buf** 在 **otherBuffer** 之前，之后或相同。

```js
var buffer1 = Buffer.from('ABC');
var buffer2 = Buffer.from('ABCD');
// 0 代表 两个buffer完全一致
var result = buffer1.compare(buffer2); // buffer1 和 buffer2 比較

if(result < 0) {	console.log(buffer1 + " 在 " + buffer2 + "之前");
}else if(result == 0){	console.log(buffer1 + " 与 " + buffer2 + "相同");
}else {	console.log(buffer1 + " 在 " + buffer2 + "之后");
}
// 输出：ABC在ABCD之前
```

------

### 拷贝缓冲区

```js
buf.copy(targetBuffer, targetStart, sourceStart, sourceEnd)
```

- **targetBuffer** - 要拷贝的 Buffer 对象。
- **targetStart** - 数字, 可选, 默认: 0
- **sourceStart** - 数字, 可选, 默认: 0
- **sourceEnd** - 数字, 可选, 默认: buffer.length

```js
var buf1 = Buffer.from('abcdefghijkl');
var buf2 = Buffer.from('RUNOOB');
// buf1 拷贝并变成了 buf2 的内容
buf2.copy(buf1);
console.log(buf1.toString());	// 輸出: RUNOOB
//将 buf2 插入到 buf1 指定位置上
buf2.copy(buf1, 2);		// ab + RUNOOB + ijkl  cdefgh被RUNOOB替换了
console.log(buf1.toString());	// 輸出: abRUNOOBijkl
```

------

#### 缓冲区裁剪

```
buf.slice(start, end)
```

- **start** - 数字, 可选, 默认: 0
- **end** - 数字, 可选, 默认: buffer.length

返回一个新的缓冲区，它和旧缓冲区指向同一块内存，但是从索引 start 到 end 的位置剪切。

```js
var buffer1 = Buffer.from('runoob');
// 裁剪 0到2区域内的内容，左开右闭
var buffer2 = buffer1.slice(0,2);
console.log("buffer2 content: " + buffer2.toString());
// buffer2 content: ru
```

------

### 缓冲区长度

```
buf.length;
```

```js
var buffer = Buffer.from('www.runoob.com');
//  缓冲区长度
console.log("buffer length: " + buffer.length);
// buffer length: 14
```



## fs文件读取模块

读取文件









## Path 路径系统模块

> path 模块是 nodejs 中用于处理文件/目录路径的一个内置模块，可以看作是一个工具箱，提供诸多方法供我们使用，当然都是和路径处理有关的。

```js
const path = require('path'); 
```



### path.basename(path, [ext])

> `path.basename()`方法：返回路径中最终的文件名称+后缀

例子：如果`path`不是一个字符串或提供了`ext`但不是一个字符串，则抛出TypeError。

```csharp
// path.basename('路径','后缀名') 
path.basename('/foo/bar/quux.html'); // 返回：‘quux.html’
path.basename('/foo/bar/quux.html', '.html'); // 返回：‘quux’
```



### path.dirname(path)

> `path.dirname()`方法：返回路径中的目录名称

例子：如果`path`不是一个字符串，则抛出TypeError。

```csharp
path.dirname('/foo/bar/baz/asdf/quux');	// 返回：‘/foo/bar/baz/asdf’
```



### path.extname(path)

> `path.extname()`方法：获取路径中文件的后缀名称(扩展名)

例子：如果`path`不是一个字符串，则抛出TypeError。

```csharp
path.extname('index.html'); // 返回：‘.html’
path.extname('index.coffee.md'); // 返回：‘.md’
path.extname('index.'); // 返回： ‘.’
path.extname('index'); // 返回：‘’
path.extname('.index'); // 返回：‘’
```



### path.parse(path)

> `path.parse()`方法返回一个对象，对象的属性表示 path 的元素。

返回的对象有以下属性：

```csharp
例如，在 POSIX 上：
path.parse('/home/user/dir/file.txt')
// {
//    root : "/",				// 根路径
//    dir : "/home/user/dir",	// dirname	文件路径
//    base : "file.txt",		// basename	文件名
//    ext : ".txt",				// extname 后缀
//    name : "file"
// }

在 Windows 上：
path.parse('C:\\path\\dir\\file.txt')
// {
//    root : "C:\\",
//    dir : "C:\\path\\dir",
//    base : "file.txt",
//    ext : ".txt",
//    name : "file"
// } 
```



### path.format(pathObject)

`path.format()`方法：传递一个对象 根据对象生成对应path地址。**与`path.parse()`相反。**

当`parhObject`提供的属性有组合时，有些属性的优先级比其他的高：

- 如果提供了`dir`,则`root`会被忽略
- 如果提供`base`存在，则`ext`和`name`会被忽略
- 其实就是 dir 加 base 去生成一个path路径

```bash
// 如果提供了'dir'、'root'和'base',则返回'${dir}$(path.sep)${base}'。
// 'root'会被忽略
path.format({
  root: '/ignored',
  dir: '/home/user/dir',
  base: 'file.txt'
});
// 返回：'/home/user/dir/file.txt'

// 如果没有指定'dir',则'root'会被使用。
// 如果只提供了'root'或'dir'等于'root',则平台的分隔符不会被包含。
// 'ext'会被忽略
path.format({
  root: '/',
  name: 'file',
  ext: '.txt' 
})
// 返回：'/file.txt'
```



### path.isAbsolute(path)

> `path.isAbsolute()`方法：判定`path`是否为一个绝对路径。
>
> 如果给定的`path`是一个长度为零的字符串，则返回`false`。
>
> 是否是 / 后缀 \ 开头

```ruby
// 在POSIX上：
path.isAbsolute('/foo/bar'); // true
path.isAbsolute('/baz/..'); // true
path.isAbsolute('qux/'); // false
path.isAbsolute('.'); // false

// 在Windows上：
path.isAbsolute('//server')    // true
path.isAbsolute('\\\\server')  // true
path.isAbsolute('C:/foo/..')   // true
path.isAbsolute('C:\\foo\\..') // true
path.isAbsolute('bar\\baz')    // false
path.isAbsolute('bar/baz')     // false
path.isAbsolute('.')           // false
```



### path.join([...paths])

> `path.join()`方法：拼接路径。

长度为零的 `path` 片段会被忽略。 如果连接后的路径字符串是一个长度为零的字符串，则返回 '.'，表示当前工作目录。

```csharp
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..')
// 返回: '/foo/bar/baz/asdf'

path.join('foo', {}, 'bar')
// 抛出 TypeError: path.join 的参数必须为字符串
```



### path.normalize(path)

> `path.normalize()` 方法：*规范路径 清除路径中的无效目录 无效内容*  *返回 标准化路径内容*

当发现多个连续的路径分隔符时（如 POSIX 上的 / 与 Windows 上的 \），它们会被单一的路径分隔符替换。 末尾的多个分隔符会被保留。

如果 path 是一个长度为零的字符串，则返回 '.'，表示当前工作目录。

在 POSIX 上：

```bash
path.normalize('/foo/bar//baz/asdf/quux/..')
// 返回: '/foo/bar/baz/asdf'
```

在 Windows 上：

```ruby
path.normalize('C:\\temp\\\\foo\\bar\\..\\');
// 返回: 'C:\\temp\\foo\\'
```



### path.relative(from, to)

path.relative(‘起始路径’，‘到什么路径’) 方法：返回从 from 到 to 的相对路径。 

如果 from 和 to 各自解析到同一路径（调用 path.resolve()），则返回一个长度为零的字符串。

如果 from 或 to 传入了一个长度为零的字符串，则当前工作目录会被用于代替长度为零的字符串。

在 POSIX 上：

```bash
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb')
// 返回: '../../impl/bbb'
```

在 Windows 上：

```rust
path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb')
// 返回: '..\\..\\impl\\bbb'
```



### path.resolve([...paths])

- ...paths <String>  一个路径或路径片段的序列
- 返回：<String>

> path.resolve() 方法：*将路径或者路径片段序列解析为绝对路径* 
>
>    *在服务器或者电脑上 锚定一个资源最稳定的方式是 获取资源的相对路径*

如果没有传入 path 片段，则 path.resolve() 会返回当前工作目录的绝对路径。

```csharp
path.resolve('/foo/bar', './baz')
// 返回: '/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/')
// 返回: '/tmp/file'

path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')
// 如果当前工作目录为 /home/myself/node，
// 则返回 '/home/myself/node/wwwroot/static_files/gif/image.gif'
```



### path.sep

> path.sep 属性 当前系统下的 默认路径符号 Windows  \\ 或者 posix /

在 POSIX 上：

```bash
'foo/bar/baz'.split(path.sep)
// 返回: ['foo', 'bar', 'baz']
```



### path.*delimiter*

> path.*delimiter*属性 *当前系统下的环境变量地址分隔符 Windows ; 或者 posix :*

