###jquery源码解读

简化jquery的源码：


```
( function( global, factory ) {
	//...兼容环境
})( typeof window !== "undefined" ? window : this, function( window, noGlobal )
```

其中，`factory`函数是`jquery`对象的实例化。

对于`global`对象，在浏览器内是为`window`，而在`Node`环境内，是`global`,因此进行如下判断：

```
// 当为Node环境时，this === global
// 判断 window 对象是否存在，若否则代入全局的 this，即 Node 环境下的 global 对象
typeof window !== "undefined" ? window : this, function( window, noGlobal )
```
`jquery` 兼容环境的做法为：

```
function( global, factory ) {

	"use strict";
	//判断是否支持CommonJs类的环境
	if ( typeof module === "object" && typeof module.exports === "object" ) {

		// 对于CommonJS和CommonJS类环境，其中有一个正确的“窗口”
		// 出现，执行工厂并获得jQuery。
		// 对于没有“文件”的“窗口”的环境
		// （如Node.js），将工厂公开为module.exports
		// 这突出了创建一个真正的“窗口”的需要
		module.exports = global.document ?
			factory( global, true ) :
			function( w ) {
				if ( !w.document ) {
					throw new Error( "jQuery requires a window with a document" );
				}
				return factory( w );
			};
	} else {
		factory( global );
	}
}
```
流程图如下：

![Alt text](./1502171150087.png)

其中，

```
if ( typeof module === "object" && typeof module.exports === "object" )
```
是为了兼容`CommonJS`规范的框架。

### `isWindow`函数

源码：

```
var isWindow = function isWindow( obj ) {
		return obj != null && obj === obj.window;
	};
```

判断是否为window对象，如果是window对象时，`window === window.window`


### `DOMEval`函数

源码：

```
	function DOMEval( code, doc ) {
		doc = doc || document;
		var script = doc.createElement( "script" );
		script.text = code;
		doc.head.appendChild( script ).parentNode.removeChild( script );
	}
```

功能：其实跟`js`的`eval`函数很相像，就是执行一端`js`代码。

学习了：

`appendChild => `
`scripr.text => `
`removeChild => `
`parentNode  =>`

### `jQuery`函数

源码：

```
jQuery = function( selector, context ) {
	return new jQuery.fn.init( selector, context );
}
```

功能：返回`jq`对象，`jq`对象有内置一些功能。

### `toArray`函数

源码：

```
function() {
	return slice.call( this );
}
```

功能：类数组转换成数组

### `get函数`

```
get: function( num ) {

		if ( num == null ) {
			return slice.call( this );
		}

		return num < 0 ? this[ num + this.length ] : this[ num ];
	}
```


