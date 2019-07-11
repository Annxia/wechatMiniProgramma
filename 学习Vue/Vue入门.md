## Vue.js是什么？
Vue 是一套用于构建用户界面的渐进式框架。
与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。
Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。
另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

## 模板语法
- {{text}}文本插值
- <div v-html="html"></div> Html输出
- v-bind HTML 属性插值。
	如<button v-bind:disabled="someDynamicCondition">Button</button>
- JavaScript 表达式。直接在 mustache、属性插值里面使用各种表达式（加减乘除、三元运算、方法调用等）。
- 过滤器（有点类似 Shell 命令中的管道，可以定义过滤器来对原始值进行变化）。
- 指令。之前提到的 v-bind 也是一种指定，其他包括 v-on: 系列（dom 事件的监听）、v-for、v-model等

