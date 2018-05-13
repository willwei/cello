---
title: VueObserver源码学习笔记
date: 2018-05-13 21:58:31
tags: 
- vue
---
# 学一下，很皮很开心

## Vue生命周期中的渲染更新流程

### Vue中响应式初始化：

```javascript
// 在初始化属性时，初始化观察者，并且遍历属性，使其成为响应属性
initState
-> initData
  -> observe(data, true)
    -> new Observer(data)
      -> walk
        -> defineReactive(obj, keys[i])
          -> new Dep () // 一个属性对应一个Dep实例
-> new Watcher (vm, updateComponent, noop) // 运行updateComponent进行求值，updateComponent返回值始终为undefined，由依赖触发其更新
  -> watcher.get() // 执行updateComponent函数，此时第一次运行render
```

### Render函数执行时，观察者收集依赖：

```javascript
// 渲染函数运行时，触发各个属性的getter
reactiveGetter[ defineReactive ]
  -> dep.depend()
    -> watcher.addDep() // 收集render中用到的各个属性对应的dep，保存到watcher.deps组数中
      -> dep.addSubs() // 此时watcher被收集为订阅者
```

### 属性被赋值时，值变更触发依赖更新：

```javascript
// 属性发生变更时，触发对应属性的setter
reactiveSetter[ defineReactive ]
  -> dep.notify() // 发布消息
    -> subs[i].update()
      -> queueWatcher
        -> flushSchedulerQueue
          -> watcher.run()
```

## 类分析

### Observer [观察者]

---
源码注释

`Observer class that are attached to each observed object. Once attached, the observer converts target object's property keys into getter/setters that collect dependencies and dispatches updates.`

解读

观察者，会为目标对象上的各个属性添加`getter/setters`, 用于收集依赖与触发更新。

#### properties

```javascript
value: any;
dep: Dep;
vmCount: number; // 某个JS对象被作为data的vm个数
```

#### methods

- constructor
- walk
- observeArray

#### 流程

```javascript
observe(data, true)
  -> new Observer(data)
    -> walk to defineReactive
  -> vmCount ++
```

### Watcher [侦听器]

---
源码中的注释
`A watcher parses an expression, collects dependencies, and fires callback when the expression value changes. This is used for both the $watch() api and directives.`

侦听器，解析一个表达式，并且收集过程中的依赖，当表达式的值发生变化时，触发回调函数。

#### properties

```javascript
vm: Component,
expression: string,
cb: Function,
id: number,
deep: boolean, // options default false
user: boolean, // options default false
computed: boolean, // options default false
sync: boolean, // options default false
dirty: boolean,
active: boolean,
dep: Dep,
deps: Array<Dep>,
newDeps: Array<Dep>,
depIds: SimpleSet,
newDepIds: SimpleSet,
before: ?Function,
getter: Function,
value: any
```

#### methods

- cleanupDeps
- update
- run
- getAndInvoke
- evaluate
- depend
- teardown

#### 流程

```javascript
// to be continued...
```

### Dep [依赖]

---
源码注释

`A dep is an observable that can have multiple directives subscribing to it. Sub array to save which properties depend on it`

解读

用于记录订阅者（Sub数组），并为每个vue实例的侦听器添加`属性依赖`

#### properties

```javascript
static target: ?Watcher,
id: number,
subs: Array<Watcher>
```

#### methods

- addSub
- removeSub
- depend
- notify

#### 流程

```javascript
// to be continued...
```