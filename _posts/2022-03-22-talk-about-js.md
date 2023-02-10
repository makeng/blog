---
layout: post
title: "关于 JS，我们聊什么？"
date: 2022-03-22
categories: js
comments: true
---


# Coding

## {Features}
### 促使 ESM 出现的理念是什么？
* 模块化编程并不少见，也不新鲜。必须要以它应对复杂的软件构建
* JS 在快速、大规模崛起，ESM 的复用、指针、tree-shaking，帮助支撑这些复杂的逻辑



### 有什么容易忽略的性能优化手段？

* 网络的 CDN、缓存、Gzip
* 防止内存溢出、图片、列表懒加载也算是优化的一种。



## {MVC}

### React 最大的性能问题是什么？怎么解决？
* setState() 语法。一是强制更新，调用了必然整个组件 render()；二是全量，状态更新是向下流动的（从 state 到 props 子组件）一整棵树
   * 用 PureComponent 可以解决，理念同纯函数，无副作用，也就是不触发子组件的更新。不过执行的是浅比较，也就是替换整个对象/数组才能更新复杂数据。
* 逻辑控制组件隐藏，显示和隐藏都是瞬间变化的。
   * 引入动画库



### Vue 是否解决了 React 的问题？
* 性能上，Vue 使用的是响应式机制。一不会强制更新，只会更新数据被触发了更新的部分；二不会触发整个子组件树的更新。
* 样式上，主要用 style hide 控制隐藏，减少了重排，且天然支持动画
* React 采用单向数据流，Vue 用指令封装了双向绑定（bind、sync），节约了代码





### Hooks 合理吗？它解决了什么问题？

* React 的核心理念一直都是简单易上手，会 JS 等于会 React。比如用 useMemo 来引入函数式中记忆化的概念，来替换独有的 API PureComponent。
* 👍 从 OOP 向 FP 迈进，作者认为，交互事件更应该偏向功能，而不是 class。
    * 虽然 OOP 支撑了 MVC 很长一段时间，但是回头看业务开发，很多时候，state 的数据之间，或者交互事件之间，并无实际联系，不如拆分。
    * 让开发者根据业务逻辑的天然的内聚性，来组织代码
* 👎 这是范式上的尝试，比较冒进。引入了更多 API，增加了学习成本。不过 API 的增加，可以减少了乱设计的情况，比如 useRef() 提醒大家别什么都扔进 state，或者动不动就把非公共方法挂在 class 上。



<br/>

# Detail
## {JS}
### JS 的诞生是个错误吗？
   * 有可能是 JS 绑架了业界的发展。但是面对缺陷，最好是优化，而不是颠覆。
   * JS 能应对高速发展的时代吗？在刀耕火种的 jQuery 年代，业务简单，JS 的坏处很显眼。但是现在业务编程进入了自动化阶段，业务需求也从普通的切图，上升到复杂的交互，JS 依然能应对，说明它适应着时代。
   * 未来（包括 ES6）的新 API 都是对过去缺陷/问题的补充，甚至有设计（TypeScript）来进行控制。



### [WebAssembly 的出现是否会取代 JavaScript？](https://www.zhihu.com/question/322007706/answer/741764049)
   * V8 几乎是这个星球上最先进的工业级脚本语言引擎。别管你代码怎么梭，只要你能让 V8 走在 Happy Path 上，那妥妥都是原生级性能。
      * 大多数页面不会到死抠性能的地步，JS 也不会在 60FPS 下运行。最高性能的原生，代价极高，损耗了性能的 Vue，开发极爽。
      * 音视频、游戏、大数据是性能的重灾区。
   * ES 规范在不断演化。这门语言向下兼容性极佳并且仍然在进化。技术圈内独家的转译玩法，让隔壁还有人为 2 还是 3 站队的时候，前端们都在用 Chrome 明年才会支持的语法特性了。
   * NPM 背后还有个巨大的娱乐圈，哦不，社区支持。
   * 🐞WebAssembly 工具链复杂、调试困难、调 OpenGL 要回到 WebGL



### 闭包的生命周期有多长？会引起内存泄漏吗？
* ⚠️ 它存在于整个程序生命中
* 用得着就不算泄漏。Vue 源码中也没有对 cache 进行回收



### 错误类型有包装对象吗？
* 实际上没有。Object.prorotype.toString() 的结果，只是看起来有那么个字符串。（总得有个精准的判断吧）
* Reflect.toString.call 更简短，效果一致：'\[object Undefined\]'、'\[object Null\]'、'\[object Boolean\]'



### 函数参数 arguments 转换后是 '\[object Object\]' 吗？
* arguments：'\[object Arguments\]'，\[…arguments\] 转成数组就是  '\[object Array\]'
* …rest：'\[object Array\]'





## {TS}
### Tyepscript 是银弹吗？
* 不是。只是增加可读性、增加严谨性的工具。
* 真正解决问题，还得看规范和 review。质量问题的重灾区，主要在设计上，极少在类型上



### class 与 interface 的异同在哪？
   * 相同：可以相互转化。interface 编译后也是对象，class 可以当成 interface 使用。
   * 不同：interface 是用于规定对象形状的，具体实现由 class 实现。在设计公共类的时候，最适合的是 class 而不是 interface。比如 Cat 继承给 Tiger 和 Leopard，而不是 interface Cat



### interface 与 type 的区别在哪？

* 相同：定义对象形状，支持合并
* 不同
   * interface：针对对象和继承，比如 readonly、protect、 extends
   * type：支持组合，应对更复杂的情况 type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;



### 给组件用 .d.ts 文件来声明类型行不行？

* 不行。虽然语法上能编译通过，但是其定义仍然是“库和全局”
* 正确是声明一个 types.ts




