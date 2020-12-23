# webpack_std

# resources
1. loader/plugin: [https://zhuanlan.zhihu.com/p/28245984]
2. tapable: [https://www.jianshu.com/p/5c87806fe8b9]
3. 手写插件: [https://www.cnblogs.com/tugenhua0707/p/11332463.html]

## webpack插件的基本原理
1. 在webpack.config.js 中引入了一个如下插件
2. 使用该插件
```
module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html' // 模版文件
    }),
  ]
};
```

## 从官网得知：编写一个webpack插件需要由以下组成
1. 一个javascript的命名函数
2. 在插件函数的prototype上定义一个apply方法
3. 指定一个绑定到webpack自身到钩子函数
4. 处理webpack内部实例到特定数据
5. 功能完成后调用webpack提供的回调函数

一个最基础的插件代码像如下这个样子:
```
// 一个javascript命名函数
function MyExampleWebpackPlugin() {

}
// 在插件函数的prototype上定义一个 apply 方法
MyExampleWebpackPlugin.prototype.apply = function(compiler) {
    // 指定一个挂载到webpack自身的事件钩子
    compiler.plugin('webpacksEventHook', function(compilation, callback) {
        console.log('这是一个插件demo')
        // 功能完成后调用 webpack 提供的回调
        callback()
    })
}
// 导出plugin
module.exports = MyExampleWebpackPlugin
```
在我们使用该plugin的时候，相关调用及配置代码如下:
```
const MyExampleWebpackPlugin = require('./MyExampleWebpackPlugin');
module.exports = {
  plugins: [
    new MyExampleWebpackPlugin(options)
  ]
};
```
1. webpack启动后，在读取配置的过程中会先执行 new MyExampleWebpackPlugin(options) 初始化MyExampleWebpackPlugin来获得一个实列
2. 然后把实例当作参数传递给compiler对象，然后实例化compiler类，在类实例中我们会获得option参数
3. 该参数是一个对象，该对象下有一个plugins这个属性，然后遍历该属性，一次执行某项插件的apply方法


## 理解 Compiler对象 和 Compilation 对象
在开发Plugin时我们最常用的两个对象就是 Compiler 和 Compilation, 他们是Plugin和webpack之间的桥梁

## Compiler对象
complier对象包含了webpack所有配置信息，options， loaders，plugins，这个对象在wepack启动的时候被实例化，它是全局唯一的，可以理解为webpack的实例，开发插件时，我们可以从compiler中拿到所有和webpack主环境相关的内容

## compilation 对象
compilation对象包含了当前的模块资源，编译生成资源，文件的变化等，当webpack在开发模式下运行时，每当检测到文件发生变化，那么一次新到compilation就会被创建，从而生成一组新到编译资源

## Compiler对象 与 Compilation 对象 的区别
compiler对象代表整个webpack从启动到关闭的生命周期，compilation对象仅仅代表了一次新的编译

## 理解webpack中的事件流
1. 可以把webpack理解为一个生产线，需要一系列的流程处理后才能将源文件转换成最后的结果
2. 这条生产线上的每个流程都是单一的，多个流程之间会存在依赖关系，只有完成当前流程后，才能进行下一步
3. 我们的插件就像是插入流程中的一个功能，在特定的时机会对资源做处理，webpack通过tapable来组织这条复杂的生产线
4. webpack在运行的过程中会广播事件，插件只需要关心监听它的事件，就能加入到生产线中，执行相关操作
5. webpack事件流机制保证了插件的有序性，使整个系统的拓展性好，事件流机制使用了观察者模式来实现