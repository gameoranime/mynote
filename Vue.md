# Vue基础整理

---

## 前言

- @author: zengjiahao
- @date: 2018/10/28

- 摘取自Vue官网文档,方便自己学习使用

## 内容目录

[TOC]



---

## 1.安装
参考链接:https://cn.vuejs.org/v2/guide/installation.html
直接下载并用 script 标签引入，Vue 会被注册为一个全局变量。
可以直接使用CDN引入：
~~~
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
~~~

## 2.Vue介绍
参考链接:https://cn.vuejs.org/v2/guide/index.html

## 3.Vue实例
参考链接:https://cn.vuejs.org/v2/guide/instance.html

### 3.1创建实例
每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的：
当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

~~~
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
~~~
以后你可以在 API 参考中查阅到完整的实例属性和方法的列表。
实例属性参考文档: https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7

### 3.2实例生命周期钩子
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。
比如 created 钩子可以用来在一个实例被创建之后执行代码：
~~~
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
~~~
也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 mounted、updated 和 destroyed。生命周期钩子的 this 上下文指向调用它的 Vue 实例。
注意:不要在选项属性或回调上使用箭头函数,因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例


## 4.模板语法-插值
参考链接: https://cn.vuejs.org/v2/guide/syntax.html

### 4.1文本

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：
~~~
    <span>Message: {{ msg }}</span>
~~~

Mustache 标签将会被替代为对应数据对象上 msg 属性的值。无论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新。

通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：
~~~
    <span v-once>这个将不会改变: {{ msg }}</span>
~~~

亦可尝试下列代码,在Vue实例的data属性中写入需要的文本
~~~
    <p v-text="message"></p>
~~~

### 4.2html格式文本

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令：

~~~
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
~~~

### 4.3相关特性
Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令：
~~~
<div v-bind:id="dynamicId"></div>
~~~
在布尔特性的情况下，它们的存在即暗示为 true，v-bind 工作起来略有不同
~~~
<button v-bind:disabled="isButtonDisabled">Button</button>
~~~
如果 isButtonDisabled 的值是 null、undefined 或 false，则 disabled 特性甚至不会被包含在渲染出来的 button 元素中。

### 4.4使用 JavaScript 表达式
迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。
~~~
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
~~~
这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。
~~~
<!-- 这是语句，不是表达式 so不会生效-->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
~~~
模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

## 5.模板语法-指令
参考链接: https://cn.vuejs.org/v2/guide/syntax.html#%E6%8C%87%E4%BB%A4
### 5.1 指令介绍
指令 (Directives) 是带有 v- 前缀的特殊特性。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

查看完整的指令:https://cn.vuejs.org/v2/api/#%E6%8C%87%E4%BB%A4

### 5.2 参数
#### v-bind 单向绑定
一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，v-bind 指令可以用于响应式地更新 HTML 特性：
~~~
<a v-bind:href="url">...</a>
~~~
在这里 href 是参数，告知 v-bind 指令将该元素的 href 特性与表达式 url 的值绑定,通过Vue实例data中的url传值。

#### v-on 绑定事件
 v-on 指令，它用于监听 DOM 事件：
~~~
<a v-on:click="doSomething">...</a>
~~~
在这里参数是监听的事件名。我们也会更详细地讨论事件处理。

### 5.3 修饰符介绍
修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
~~~
<form v-on:submit.prevent="onSubmit">...</form>
~~~

### 5.4 参数缩写
v-bind:特性 => :特性
~~~
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
~~~
v-on:事件名 => @事件名
~~~
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
~~~

## 6.条件渲染
参考链接: https://cn.vuejs.org/v2/guide/conditional.html
### 6.1 v-if v-else 指令
在 Vue 中，我们使用 v-if 指令实现同样的功能：
也可以用 v-else 添加一个“else 块”：
~~~
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
~~~
v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。

### 6.2 v-else-if 指令
~~~
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
~~~
类似于 v-else，v-else-if 也必须紧跟在带 v-if 或者 v-else-if 的元素之后。

### 6.3 v-show
另一个用于根据条件展示元素的选项是 v-show 指令。用法大致一样：
~~~
<h1 v-show="ok">Hello!</h1>
~~~
不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
注意: v-show 不支持 template 元素，道理很简单,因为template并不会以DOM形式显示 , 也不支持 v-else。

### 6.4 v-if 与 v-show 区别[常用,不管是面试还是开发]

v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

### 6.5 在 template 元素上使用 v-if 条件渲染分组
因为 v-if 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 template 元素当做不可见的包裹元素，并在上面使用 v-if。最终的渲染结果将不包含 template 元素。

### 6.6 用 key 管理可复用的元素
参考链接:https://cn.vuejs.org/v2/guide/conditional.html#%E7%94%A8-key-%E7%AE%A1%E7%90%86%E5%8F%AF%E5%A4%8D%E7%94%A8%E7%9A%84%E5%85%83%E7%B4%A0
详细看参考链接,这里简述
Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。
那么,复用已存在的元素会导致一些问题,如input框本是输入名字,后面由于某些操作,将输入名字的框改成了输入邮箱的框,但是由于复用了该input框,结果,里面的值没有清除.
这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key 属性即可：
详细看参考链接

### 6.7 补充 v-if 与 v-for 一起使用的问题[避免]
不推荐同时使用 v-if 和 v-for。请查阅 风格指南 以获取更多信息。
当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。请查阅 列表渲染指南 以获取详细信息。
避免即可,实际开发用到,再深入研究
参考链接1列表渲染指南:https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if
参考链接2风格指南:https://cn.vuejs.org/v2/style-guide/#%E9%81%BF%E5%85%8D-v-if-%E5%92%8C-v-for-%E7%94%A8%E5%9C%A8%E4%B8%80%E8%B5%B7-%E5%BF%85%E8%A6%81


## 7.列表渲染
参考链接:https://cn.vuejs.org/v2/guide/list.html

### 7.1基础使用
#### 用 v-for 把一个数组对应为一组元素
我们用 v-for 指令根据一组数组的选项列表进行渲染。v-for 指令需要使用 item in items 形式的特殊语法，items 是源数据数组并且 item 是数组元素迭代的别名。
~~~
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
~~~
在 v-for 块中，我们拥有对父作用域属性的完全访问权限。v-for 还支持一个可选的第二个参数为当前项的索引。
~~~
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
~~~
你也可以用 of 替代 in 作为分隔符，因为它是最接近 JavaScript 迭代器的语法：
~~~
<div v-for="item of items"></div>
~~~
#### 一个对象的 v-for
你也可以用 v-for 通过一个对象的属性来迭代。

~~~
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
~~~
你也可以提供第二个的参数为键名：
~~~
<div v-for="(value, key) in object">
  {{ key }}: {{ value }}
</div>
~~~
第三个参数为索引：
~~~
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
~~~

#### 关于 v-for 中的 :key
当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。

这个默认的模式是高效的，但是只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。

建议尽可能在使用 v-for 时提供 key，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
因为它是 Vue 识别节点的一个通用机制，key 并不与 v-for 特别关联，key 还具有其他用途

### 7.2数组更新检测
参考链接:https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B
详情请看参考链接,这里简述
#### 变异方法
Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：
push()
pop()
shift()
unshift()
splice()
sort()
reverse()

#### 替换数组
属于非变异方法,这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组

#### 解释误区
你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的、启发式的方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。

### 7.3注意事项
#### 数组更新注意事项
由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
1. 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
2. 当你修改数组的长度时，例如：vm.items.length = newLength

为了解决第一类问题，以下两种方式都可以实现
​    Vue.set(vm.items, indexOfItem, newValue)
​    vm.items.splice(indexOfItem, 1, newValue)
为了解决第二类问题，你可以使用 splice：
​    vm.items.splice(newLength)

详情例子:请看文档
参考链接:https://cn.vuejs.org/v2/guide/list.html#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9

#### 对象更改检测注意事项
还是由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除：
~~~
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 现在是响应式的

vm.b = 2
// `vm.b` 不是响应式的
~~~
对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性。
有时你可能需要为已有对象赋予多个新属性，比如使用 Object.assign() 或 _.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。
~~~
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
~~~

### 7.4显示过滤/排序结果
有时，我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。
~~~
<li v-for="n in evenNumbers">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
~~~
在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法：(引用上例)
~~~
<li v-for="n in even(numbers)">{{ n }}</li>
~~~
### 7.5v-for补充说明[用到时查文档]
#### 一段取值范围的 v-for
v-for 也可以取整数。在这种情况下，它将重复多次模板。
参考链接: https://cn.vuejs.org/v2/guide/list.html#%E4%B8%80%E6%AE%B5%E5%8F%96%E5%80%BC%E8%8C%83%E5%9B%B4%E7%9A%84-v-for
#### v-for on a <template>
类似于 v-if，你也可以利用带有 v-for 的 template 渲染多个元素。
参考链接: https://cn.vuejs.org/v2/guide/list.html#v-for-on-a-lt-template-gt
#### v-for with v-if 使用
当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当你想为仅有的一些项渲染节点时，这种优先级的机制会十分有用，如下：
参考链接: https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if
#### 一个组件的 v-for
参考链接: https://cn.vuejs.org/v2/guide/list.html#%E4%B8%80%E4%B8%AA%E7%BB%84%E4%BB%B6%E7%9A%84-v-for

## 8.事件处理
参考链接:https://cn.vuejs.org/v2/guide/events.html

### 8.1事件监听
可以用 v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

### 8.2事件处理方法
然而许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 v-on 指令中是不可行的。因此 v-on 还可以接收一个需要调用的方法名称。
~~~
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>
~~~

#### 内联处理器中的方法(调用方法传参)
除了直接绑定到一个方法，也可以在内联 JavaScript 语句中调用方法：
~~~
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
~~~
有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 $event 把它传入方法：
~~~
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

// ...
methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) event.preventDefault()
    alert(message)
  }
}
~~~

### 8.3事件修饰符
参考链接:https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6
修饰符是由点开头的指令后缀来表示的,通常阻止默认行为等会用到,详细看文档或查阅网上资料
.stop
.prevent
.capture
.self
.once
.passive
```
<!-- 阻止单击事件继续传播(即阻止冒泡) -->
<a v-on:click.stop="doThis"></a>
```
#### 使用事件修饰符的注意点
1. 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。
2. 不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，.passive 会告诉浏览器你不想阻止事件的默认行为。

### 8.4按键修饰符
在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：
```
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
```
记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right
```
<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```
可以通过全局 config.keyCodes 对象自定义按键修饰符别名：
```
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

#### 自动匹配按键修饰符
参考链接:https://cn.vuejs.org/v2/guide/events.html#%E8%87%AA%E5%8A%A8%E5%8C%B9%E9%85%8D%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6

### 8.5系统修饰键
参考链接:https://cn.vuejs.org/v2/guide/events.html#%E7%B3%BB%E7%BB%9F%E4%BF%AE%E9%A5%B0%E9%94%AE
.ctrl
.alt
.shift
.meta
请注意修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。而单单释放 ctrl 也不会触发事件。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17。

#### .exact 修饰符
.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件,看例子就懂了
```
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

#### 鼠标按钮修饰符
这些修饰符会限制处理函数仅响应特定的鼠标按钮。
.left
.right
.middle

#### 为什么在 HTML 中监听事件?[了解,面试题]
参考链接:https://cn.vuejs.org/v2/guide/events.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%9C%A8-HTML-%E4%B8%AD%E7%9B%91%E5%90%AC%E4%BA%8B%E4%BB%B6

## 9.v-model表单输入绑定
参考链接:https://cn.vuejs.org/v2/guide/forms.html 

### 9.1基础用法介绍
你可以用 v-model 指令在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

#### 注意点
1. v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。
  你应该通过 JavaScript 在组件的 data 选项中声明初始值。
2. 对于需要使用输入法 (如中文、日文、韩文等) 的语言，你会发现 v-model 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 input 事件。

### 9.2基础使用

#### 文本

```
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

#### textarea多行文本
```
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```
在文本区域插值 (<textarea></textarea>) 并不会生效，应用 v-model 来代替。

#### checkbox复选框
单个复选框，绑定到布尔值,实例data获取到复选框的布尔值：
```
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```
多个复选框，绑定到同一个数组,选中项会加入到数组：

#### radio单选按钮
监听单选按钮的value值
```
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
```

#### select选择框（单选、多选）
##### 监听选择框选项（单选情况）
```
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```
注意事项：
如果 v-model 表达式的初始值未能匹配任何选项，<select> 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。

##### 监听选择框选项（多选情况，需绑定到一个数组）
上述单选例子 在 select标签里 加上 multiple style="width: 50px;"
开启多选，以及调整下样式
点击选项，可将值存进vue实例数组中

##### 用 v-for 渲染的动态选项
```
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```
```
data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
```

### 值绑定（对上述基本用法补充说明）
参考链接：https://cn.vuejs.org/v2/guide/forms.html#%E5%80%BC%E7%BB%91%E5%AE%9A
对于单选按钮，复选框及选择框的选项，v-model 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：
但是有时我们可能想把值绑定到 Vue 实例的一个动态属性上，这时可以用 v-bind 实现，并且这个属性的值可以不是字符串。

#### 复选框
```
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```
```
// 当选中时
vm.toggle === 'yes'
// 当没有选中时
vm.toggle === 'no'
```
这里的 true-value 和 false-value 特性并不会影响输入控件的 value 特性，因为浏览器在提交表单时并不会包含未被选中的复选框。如果要确保表单中这两个值中的一个能够被提交，(比如“yes”或“no”)，请换用单选按钮。

#### 单选按钮
```
<input type="radio" v-model="pick" v-bind:value="a">
```
```
// 当选中时
vm.pick === vm.a
```

#### 选择框的选项
如果option选项有value值则：
select 中 v-model绑定的是 option选项的value值，可以绑对象，如果要获取其值，使用对象点语法点出
option没有value值时，value默认是option里的内容
```
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```
```
// 当选中时
typeof vm.selected // => 'object'
vm.selected.number // => 123
```

### 修饰符的使用
#### .lazy
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步：
```
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```
#### .number
如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：
```
<input v-model.number="age" type="number">
```
这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 parseFloat() 解析，则会返回原始的值。

#### .trim
如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：
```
<input v-model.trim="msg">
```

### 在组件上使用 v-model
参考链接：https://cn.vuejs.org/v2/guide/forms.html#%E5%9C%A8%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-v-model

## 10.计算属性和侦听器
参考链接：https://cn.vuejs.org/v2/guide/computed.html
### 10.1计算属性
模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。
所以，对于任何复杂逻辑，你都应当使用计算属性。
#### 简单示例
```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```
结果：

Original message: "Hello"

Computed reversed message: "olleH"

这里我们声明了一个计算属性 reversedMessage。我们提供的函数将用作属性 vm.reversedMessage 的 getter 函数：
```
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```
你可以像绑定普通属性一样在模板中绑定计算属性。
Vue 知道 vm.reversedMessage 依赖于 vm.message，因此当 vm.message 发生改变时，所有依赖 vm.reversedMessage 的绑定也会更新。
而且最妙的是我们已经以声明的方式创建了这种依赖关系：计算属性的 getter 函数是没有副作用 (side effect) 的，这使它更易于测试和理解。

### 10.2计算属性缓存 vs 方法
你可能已经注意到我们可以通过在表达式中调用方法来达到同样的效果：
```
<p>Reversed message: "{{ reversedMessage() }}"</p>
```
```
// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。
然而，不同的是计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。
这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。
这也同样意味着下面的计算属性将不再更新，因为 Date.now() 不是响应式依赖：
```
computed: {
  now: function () {
    return Date.now()
  }
}
```
相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。

我们为什么需要缓存？假设我们有一个性能开销比较大的计算属性 A，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 A 。如果没有缓存，我们将不可避免的多次执行 A 的 getter！如果你不希望有缓存，请用方法来替代。

总结：
1. 计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。
2. 方法没有缓存，调用即执行
3. 缓存有助提高性能，减少计算开销

### 10.3计算属性 vs 侦听属性
参考链接： https://cn.vuejs.org/v2/guide/computed.html#%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7-vs-%E4%BE%A6%E5%90%AC%E5%B1%9E%E6%80%A7
Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性。
当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch——特别是如果你之前使用过 AngularJS。然而，通常更好的做法是使用计算属性而不是命令式的 watch 回调。

### 10.4计算属性的 setter
计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：
```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```
现在再运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。

### 10.5侦听器
参考链接：https://cn.vuejs.org/v2/guide/computed.html#%E4%BE%A6%E5%90%AC%E5%99%A8
虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。
这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

## 11.Class 与 Style 绑定
参考链接：https://cn.vuejs.org/v2/guide/class-and-style.html
操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是属性，所以我们可以用 v-bind 处理它们：只需要通过表达式计算出字符串结果即可。
不过，字符串拼接麻烦且易错。
因此，在将 v-bind 用于 class 和 style 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。
### 11.1绑定 HTML Class
#### 绑定 HTML Class的对象语法
我们可以传给 v-bind:class 一个对象，以动态地切换 class：
```
<div v-bind:class="{ active: isActive }"></div>
```
上面的语法表示 active 这个 class 存在与否将取决于数据属性 isActive 的 truthiness。
你可以在对象中传入更多属性来动态切换多个 class。此外，v-bind:class 指令也可以与普通的 class 属性共存。当有如下模板:
```
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```
和如下data：
```
data: {
  isActive: true,
  hasError: false
}
```
结果渲染为：
```
<div class="static active"></div>
```
当 isActive 或者 hasError 变化时，class 列表将相应地更新。例如，如果 hasError 的值为 true，class 列表将变为 "static active text-danger"。


绑定的数据对象不必内联定义在模板里：
```
<div v-bind:class="classObject"></div>
```
```
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
渲染的结果和上面一样。我们也可以在这里绑定一个返回对象的计算属性。这是一个常用且强大的模式：
```
<div v-bind:class="classObject"></div>
```
```
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

#### 绑定 HTML Class的数组语法

我们可以把一个数组传给 v-bind:class，以应用一个 class 列表：
```
<div v-bind:class="[activeClass, errorClass]"></div>
```

```
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
渲染为：
```
<div class="active text-danger"></div>
```
如果你也想根据条件切换列表中的 class，可以用三元表达式：
```
<div v-bind:class="[isActive ? activeClass : '' , errorClass]"></div>
```
这样写将始终添加 errorClass，但是只有在 isActive 是 truthy[1] 时才添加 activeClass。
不过，当有多个条件 class 时这样写有些繁琐。所以在数组语法中也可以使用对象语法：
```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### 11.2class用在组件上
参考链接：https://cn.vuejs.org/v2/guide/class-and-style.html#%E7%94%A8%E5%9C%A8%E7%BB%84%E4%BB%B6%E4%B8%8A
当在一个自定义组件上使用 class 属性时，这些类将被添加到该组件的根元素上面。这个元素上已经存在的类不会被覆盖。
例如，如果你声明了这个组件：
```
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```
然后在使用它的时候添加一些 class：
```
<my-component class="baz boo"></my-component>
```
HTML 将被渲染为:
```
<p class="foo bar baz boo">Hi</p>
```
对于带数据绑定 class 也同样适用：
```
<my-component v-bind:class="{ active: isActive }"></my-component>
```
当 isActive 为 truthy[1] 时，HTML 将被渲染成为：
```
<p class="foo bar active">Hi</p>
```

### 11.3绑定内联样式style
#### 内联样式的对象语法
v-bind:style 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来) 来命名：
```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
```
data: {
  activeColor: 'red',
  fontSize: 30
}
```
直接绑定到一个样式对象通常更好，这会让模板更清晰：
```
<div v-bind:style="styleObject"></div>
```
```
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
同样的，对象语法常常结合返回对象的计算属性使用。

#### 内联样式的数组语法
v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上：
```
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

#### 自动添加前缀
当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

#### 多重值
从 2.3.0 起你可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：
```
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。


