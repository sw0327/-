## HTML&CSS

**`display: none`**：**不会移除 DOM 节点**。它只会让元素在页面上不可见，并且不占用空间，但该元素仍然存在于 DOM 结构中。

**`visibility: hidden`**：元素不可见，但仍占据原本的空间，影响页面布局。







### **浅拷贝与深拷贝**

​        浅拷贝：将原对象或原数组的引用直接赋给新对象、新数组，新对象只是对原对象的一个引用，而不复制对象本身，新旧对象还是共享同一块内存。(拓展运算符。。。)（拷贝的是地址，适合拷贝单层的数据）
​        深拷贝：开辟一个新的栈，两个对象的属性完全相同，但是对应两个不同的地址，修改一个对象的属性，不会改变另一个对象的属性。（深拷贝指的是创建一个新的指针，同时也会生成一个新的内存地址，这个新的指针指向新的内存地址。与原对象是相互独立，互不影响）

### route和router的区别详解

​    **router**是用来**操作路由**的。$router.push('')

​    **route**是用来**获取路由信息**的。 $route.path $route.[params](https://so.csdn.net/so/search?q=params&spm=1001.2101.3001.7020) route.query等

### 防抖和节流

**防抖（debounce）**：单位时间内，频繁触发事件，只执行最后一次。

应用场景：搜索框、输入框、手机邮箱验证等

思路：利用定时器，每次触发先清除定时器（）

```
const box = document.querySeleter(".box")
let i = 1
function mouseMove() {
	box.innerHTML = i++
}
function debounce(fn, t) {
	let timer
	// return 返回一个匿名函数
	return function() {
		if(timer) clearTimeout(timer)
		timer = setTimeOut(function() {
			fn()
		}, t)
	}
}
box.addEventListener('mousemove', debounce(mouseMove, 500))
```

**节流（throttle）**：单位时间内，频繁触发事件，只执行一次

应用场景比较多的是：鼠标经过、页面缩放、滚动条滚动scroll事件、下拉刷新等高频事件
思路：利用定时器，判断是否有定时器，如果有等定时器结束再开启新的定时器

```cmd
const box = document.querySeleter(".box")
let i = 1
function mouseMove() {
	box.innerHTML = i++
}
function throttle(fn, t) {
	let timer
	// return 返回一个匿名函数
	return function() {
		if(!timer) {
			timer = setTimeOut(function() {
				fn()
				// 清空定时器 此处使用timer=null清除定时器是因为写在了定时器里面，setTimeout中是无法清除定时器的，因为定时器还在运作
				timer = null
			}, t)	
		}
	}
}
box.addEventListener('mousemove', throttle(mouseMove, 500))

```

### js闭包

闭包就是能够读取其他函数内部变量的函数。

```cmd
function fn0 () {
	const aaa = 0
	return function() {
		return aaa
		console.log('打印', aaa)
	}
}
```

好处：可以读取其他函数的内部变量

​	    将变量始终保存在内存中

​	    可以封装对象的私有属性和方法

坏处：消耗内存、使用不当会造成内存溢出问题











## Vue

### vue中的data为什么是一个函数？

Vue 中的 data 必须是个函数，因为当 data 是函数时，组件实例化的时候这个函数将会被调用，返回一个对象，

计算机会给这个对象分配一个内存地址，实例化几次就分配几个内存地址，
他们的地址都不一样，所以每个组件中的数据不会相互干扰，改变其中一个组件的状态，其它组件不变。

简单来说，就是为了保证组件的独立性和可复用性，如果 data 是个函数的话，
每复用一次组件就会返回新的 data，类似于给每个组件实例创建一个私有的数据空间，保护各自的数据互不影响

- 如果data是一个函数的话，这样`每复用一次组件，就会返回一份新的data`(类似于给每个组件实例创建一个私有的数据空间，让各个组件实例维护各自的数据)，保证了组件的独立性和可复用性
- Object是引用数据类型，`里面保存的是内存地址`，单纯的`写成对象形式`，就`使得所有组件实例共用了一份data`，就会造成一个变了全都会变的结果。

### v-if 和 v-show的区别

#### **`v-if`：按需渲染**

`v-if` 只有在条件为 `true` 时，才会渲染和创建对应的 DOM 元素。当条件为 `false` 时，Vue 将不渲染这个元素，或者在条件变为 `false` 时，会销毁对应的 DOM 元素（DOM 节点完全不会存在）。

- **适用场景**：适合那些**不经常切换**显示/隐藏的元素，因为每次切换都会涉及到 DOM 的创建和销毁。

#### **`v-show`：通过 `display` 控制显示**

`v-show` 会始终渲染并存在于 DOM 中，但它通过 `CSS` 的 `display` 属性来控制元素的显示或隐藏。如果条件为 `false`，Vue 仅仅设置该元素的 `display` 为 `none`，并不会移除它的 DOM 节点。

- **适用场景**：适合那些需要**频繁切换**显示/隐藏状态的元素，因为只改变 `display` 属性而不销毁 DOM 元素。

### Vue响应式

Vue 在监听到数据变化时，并不会立即同步更新 DOM，而是先把这些变化记录下来，然后在**同一个事件循环**中，**异步地批量更新 DOM**。这种方式可以避免数据频繁变动时导致的多次不必要的 DOM 更新，从而提高性能。



在 Vue 3 中，`await nextTick()` 是一种异步方式等待 DOM 更新完成。这是 Vue 3 对异步更新机制的一种改进，它允许你在响应式数据更新后等待 DOM 更新完成再继续执行逻辑。



### event loop（事件循环）

JavaScript 是一种单线程语言，它依赖事件循环来处理同步和异步任务，以确保代码按正确的顺序执行。

#### JS是单线程

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。防止代码阻塞，我们把代码(任务) 分为同步和异步。

#### 同步任务和异步任务

- **同步任务**：在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
- **异步任务**：不进入主线程、异步任务交给宿主环境，等待时机成熟送入“任务队列”排队。只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

#### 宏任务&微任务

异步任务也有区别，分为：macrotask（宏任务） 和 microtask（微任务）

当宏任务和微任务都处于 任务队列（Task Queue） 中时，微任务的优先级大于宏任务，即先将微任务执行完，再执行宏任务；

Promise本身同步, then/ catch的回调函数是异步的微任务

| 宏任务 | setTimeout 、setInterval 、UI rendering |
| ------ | --------------------------------------- |
| 微任务 | promise.then 、requestAnimationFrame    |

#### 执行顺序：

1. 执行所有的**同步任务**。
2. 执行**微任务队列**中的所有任务，直到队列清空。
3. 如果微任务队列清空后有待执行的宏任务，则执行下一个宏任务。
4. 重复上述过程。

执行栈执行完毕，会去任务队列看是否有异步任务，有就送到执行栈执行，反复循环查看执行，这个过程是事件循环(eventloop)



### $nextTick的作用

Vue的响应式并不是数据发生变化后，DOM立即跟着发生变化的，而是按一定的策略进行DOM更新的。

作用：$nextTick 是在下次 DOM 更新循环结束之后执⾏延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM，在下次 DOM 更新循环结束之后执行延迟回调

说白了就是：当数据更新了，在DOM更新后，自动执行该函数

##### 什么时候用？

- Vue⽣命周期的created()钩⼦函数进⾏的DOM操作⼀定要放在Vue.nextTick()的回调函数中，原因是在created()钩⼦函数执⾏的时候,DOM 其实并未进⾏任何渲染，⽽此时进⾏DOM操作⽆异于徒劳，所以此处⼀定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。
- 当项⽬中改变data函数的数据，想基于新的dom做点什么，对新DOM⼀系列的js操作都需要放进Vue.nextTick()的回调函数中。

### Vue生命周期

![img](https://ask.qcloudimg.com/http-save/yehe-8223537/f4e1196839906dd54adf595b2432f8ca.jpeg)

![img](https://ask.qcloudimg.com/http-save/yehe-8223537/e692b0fa12f033538242defebb54feed.jpeg)