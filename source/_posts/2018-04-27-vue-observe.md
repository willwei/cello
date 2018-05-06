---
title: VueObserver源码学习笔记
date: 2018-05-05 21:17:31
tags: 
- vue
---
源码学习笔记，还没学习完，还在这张纸上，等自己再划拉划拉

![](/images/note-vue.jpg)

## 基本流程

1.初始化流程：

```javascript
initState
-> observe
-> new Observer
  -> walk
    -> defineReactive
      -> new Dep ()
-> new Watcher (vm, updateComponent, noop)
```

2.依赖收集：

```javascript
reactiveGetter[ defineReactive ] 
  -> dep.depend()
```

3.更新流程：

```javascript
reactiveSetter -> dep.notify() -> subs[i].update()
-> queueWatcher -> flushSchedulerQueue -> watcher.run()
-> watcher.get()
```

## Demo

...

## 源码分析（摘抄）

### Watcher

A watcher parses an expression, collects dependencies, and fires callback when the expression value changes. This is used for both the $watch() api and directives.

#### properties

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

#### methods

- cleanupDeps
- update
- run
- getAndInvoke
- evaluate
- depend
- teardown

### Dep

A dep is an observable that can have multiple directives subscribing to it. Sub array to save which properties depend on it

#### properties

static target: ?Watcher,
id: number,
subs: Array<Watcher>

#### methods

- addSub
- removeSub
- depend
- notify

### Observer

Observer class that are attached to each observed object. Once attached, the observer converts target object's property keys into getter/setters that collect dependencies and dispatches updates.

Walk properties to defineReactive

methods:

- constructor
- walk

helps:

- protoAugment
- copyAugment
- observe
- defineReactive
- set
- del
- dependArray

observe

Attempt to create an observer instance for a value,returns the new observer if successfully observed,or the existing observer if the value already has one.

流程：

observe
 -> new Observer
 -> walk to defineReactive

```javascript
function observe (value: any, asRootData: ?boolean): Observer | void {

}
```

defineReactive

Define a reactive property on an Object.

```javascript
function defineReactive (
  obj: Object,
  key:string,
  val: any,
  customSetter?: ?Function,
  shallow?: boolean
) {
  var dep = new Dep()

  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true
    get: function reactiveGetter () {
      const value = val
      if (Dep.target) {
        dep.depend()
      }
      return value
    },
    set: function reactiveSetter (newVal) {
      dep.notify()
    }
  })

}
```

result

```javascript
{
  __ob__: {
    dep: {
      id: 0
        subs: []
    },
    value: 0
    vmCount: 1
  }
}
```