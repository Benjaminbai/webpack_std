webpack 是一个模块打包器，提供了一个核心，核心提供了很多开箱即用的功能
同时它可以用loader和plugin来扩展
webpack本身结构精巧，基于tapable插件架构，拓展性强

webpack常用的配置包括entry，output，module，resolve，loader，plugin，external，devtool等

webpack常用等loader
    样式： style-loader，css-loader，less-loader，sass-loader
    文件： row-loader，file-loader，url-loader
    编译： babel-loader，coffee-loader，ts-loader
    校验测试： mocha-loader，jshint-loader，eslint-loader

    coffee-loader、babel-loader等可以将特定文件格式转成js模块、将其他语言转化为js语言和编译下一代js语言

    file-loader、url-loader等可以处理资源，file-loader可以复制和放置资源位置，并可以指定文件名模板，用hash命名更好利用缓存， raw-loader可以将文件已字符串的形式返回

webpack常用等plugin
    首先webpack内置了uglify-js-plugin，压缩和混淆代码啊
    webpack内置common-chunk-plugin，提高打包效率，将第三方库和业务代码分开打包
    html-webpack-plugin可以根据模版自动生成html代码，并自动引用css，js
    optimize-css-assets-webpack-plugin不同组件中重复等css可快速去重
    compression-webpack-plugin生成环境可采用gzip压缩css和js
    happypack通过多进程模型，加速代码构建


