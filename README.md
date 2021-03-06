### 1.数组扁平化；
* 方法一：递归
```
function flatten(arr){
	var res = [];
	//判断数组的自相是否是数组，如果是则递归，反之push
	if(Array.isArray(arr[i])){
	    res = res.concat(flatten(arr[i]))
	}else{
	    res.push(arr[i];
	}
}
return res;
const arr = [1,[2,3,[4]],5]
console.log('扁平化后：',flatten(arr));

```
* 方法二：reduce()
```
function flatten(arr){
	return arr.reduce(function(prev,item){
		return prev.concat(Array.isArray(item)?flatten(item):item)
	},[])
}
const arr = [1,[2,3,[4]],5]
console.log('扁平化后：',flatten(arr));

```
* 方法三：...
```
function flatten(arr){
	return [].concat(
		...arr.map(x => Array.isArray(x)? flatten(x):x)
	)
}
const arr = [1,[2,3,[4]],5]
console.log('扁平化后：',flatten(arr));

```

### 2.面向对象编程与面向过程编程的区别

面向对象和面向过程并不是完全相对的、完全独立的，是相互相成的关系；

面向对象：针对对象来进行执行某些动作，对象有自己的属性和方法。

面向过程：完成一个事件就是将不同的动作函数按需求调用

### 3.如何实现继承跟封装

封装的几种方式：构造函数、原型prototype、闭包实现的封装

继承：
* 类式继承：通过子类的原型prototype对象实例化来实现的。缺点就是：一个子类的实例原型从父类构造函数中继承来的共有属性，一个子类的实例如果更改了这个引用类型就会直接影响到其他子类

* 构造函数式继承：通过在子类的构造函数作用环境中执行一次父类的构造函数来实现的。通过call/apply/bind直接改变this的指向，使通过this创建的属性和方法在子类中复制一份，因为是单独复制的，所以各个实例化的子类互不影响。但是会造成内存浪费的问题。由于这种类型的继承没有涉及原型prototype,所以父类的原型方法自然不会被子类继承，而如果要想被子类继承就必须要放在构造函数中。

* 组合继承：类式继承+构造函数继承,汲取两者的优点，即避免了内存浪费，又使得每个实例化的子类互不影响

* 寄生式继承：通过在一个函数内的过渡对象实现继承并返回新对象的方式，称之为寄生式继承。寄生就像寄生虫一样寄托于某个对象内部生长。就是对原型继承的第二次封装，并且在这第二次封装过程中对继承的对象进行了扩展，这样新创建的对象不仅仅有父类中的属性和方法而且还添加了新的属性和方法。

* 寄生组合式继承：寄生式继承+构造函数式继承，先创建了父类，还有父类的原型方法，然后创建子类，并在构造函数中实现构造函数式继承，然后又通过寄生式继承了父类 原型，最后又对子类添加了一些原型方法。


### 4.sass的计算属性对页面性能有无影响

sass只是中间形态，最终执行的是css

### 5.vue2.0跟vue3.0分别是如何实现双向绑定的

vue2.0 是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

具体步骤：

第一步：需要observe的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化

第二步：compile解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

第三步：Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是:1、在自身实例化时往属性订阅器(dep)里面添加自己2、自身必须有一个update()方法3、待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退。

第四步：MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。


vue3.0 将带来一个基于 Proxy 的 observer 实现，它可以提供覆盖语言 (JavaScript——译注) 全范围的响应式能力，消除了当前 Vue 2 系列中基于 Object.defineProperty 所存在的一些局限，这些局限包括：

对属性的添加、删除动作的监测；

对数组基于下标的修改、对于 .length 修改的监测；

对 Map、Set、WeakMap 和 WeakSet 的支持；

另外，新的 observer还提供了如下的一些特性：
公开的用于创建 observable 的 API：这为小型到中型的应用提供了一种轻量级的、极其简单的跨组件状态管理解决方案。

默认为惰性监测（Lazy Observation）。在 2.x版本中，任何响应式数据，不管它的大小如何都会在启动的时候监测功能。如果数据量很大的话，在应用启动的时候就可能造成严重的性能消耗。而在3.x 版本中，只有应用的初始可见部分所用到的数据会被监测，更不用说这种监测方案本身其实也是更加快的。

### 6.Object.definedProperty（）和proxy的区别

Object.defineProperty() 的问题主要有三个：

* 不能监听数组的变化
* 必须遍历对象的每个属性
* 必须深层遍历嵌套的对象


proxy：
* 针对对象:针对整个对象,而不是对象的某个属性
* 支持数组:不需要对数组的方法进行重载，省去了众多 hack
* 嵌套支持: get 里面递归调用 Proxy 并返回

7.vue hash路由跟history路由的区别

8.vue 的nexttick实现原理

9.webpack打包过程

### 10.webpack 热部署的原理

热更新开启后，当webpack打包时，会向客户端注入一段HMR runtime代码，同时服务器端（webpack-dev-server或webpack-hot-middleware)启动一个HMR服务器，它通过webSocket和注入的runtime进行通信。
当webpack检测到文件修改后，会重新构建，并通过webSocket向客户端发送更新消息，浏览器通过jsonp拉取更新过的模块，回调触发模块热更新逻辑

* 修改了一个或多个文件
* 文件系统接收更改并通知webpack
* webpack重新编译构建一个或多个模块，并通知HMR服务器进行更新
* HMR Server 使用webSocket通知HMR runtime 需要更新，HMR运行时通过HTTP请求更新jsonp
* HMR运行时替换更新中的模块，如果确定这些模块无法更新，则触发整个页面刷新


11：webpack打包过慢跟打包出来文件体积过大怎么处理

未完待续...
