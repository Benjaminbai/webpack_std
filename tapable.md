
webpack本质上是一种事件流机制，它等工作流程就是将个个插件串联起来，而实现这一切等核心就是tapable
webpack中最核心的，负责编译的compiler，负责创建boundles的compilation都是tapable的构造函数的实例

```
const {
    SyncHook,
    SyncBailHook,
    SyncWaterfallHook,
    SyncLoopHook,
    AsyncParallelHook,
    AsyncParallelBailHook,
    AsyncSeriesHook,
    AsyncSeriesBailHook,
    AsyncSeriesWaterfallHook
 } = require("tapable");
```
上面实现事件流机制的钩子可以分成两类，“同步”和“异步”，异步又分为两类，“并行”和“串行”，而同步的钩子都是串行