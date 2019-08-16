# 个人随笔
记录工作中遇到的个人觉得比较有意思的小问题

#### js模块模式的两个必要条件
- 必须有外部的封闭函数，该函数必须被调用一次
- 封闭函数必须返回至少一个内部函数，这样内部函数才能在私有作用域形成闭包，并且可以访问或者修改私有状态。

现代模块机制的核心概念
```
var myModule = (function Manager() {
  var modules = {};
  function define(name, deps, impl) {
    for (var i = 0; i < deps.length; i++) {
      deps[i] = modules[deps[i]];
    }
    modules[name] = impl.apply(impl, deps);
  }
  
  function get(name) {
    return modules[name];
  }
  
  return {
    define: define,
    get: get,
  };
})();

myModule.define('bar', [], function() {
  function hello(who) {
    return 'Let me introduce: ' + who;
  }
  
  return {
    hello: hello,
  };
});

myModule.define('foo', ['bar'], function(bar) {
  var hungry = 'hippo';
  
  function awesome() {
    console.log( bar.hello(hungry).toUpperCase() );
  }
  
  return {
    awesome: awesome,
  };
});

var bar = myModule.get('bar');
var foo = myModule.get('foo');

console.log(
  bar.hello('wang1dot0');
);
foo.awesome();
```
ES6为模块增加了一级语法支持。将每个文件都作为一个独立的模块，每个模块内部都可以导入其他模块或者API，同样也可以导出自己的API方法。
ES6模块可以在编译期检查对导入模块的API方法的引用是否真是存在。而早期的模块机制是在运行期才能动态发现。
ES6模块在独立的文件中。浏览器或引擎有一个默认的“模块加载器”可以在导入模块时异步地加载模块文件。
