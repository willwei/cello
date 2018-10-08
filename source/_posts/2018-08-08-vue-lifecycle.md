---
title: VueLifeCycle源码学习笔记
date: 2018-08-10 21:58:31
tags: 
- vue
---
# lifecycle

## mountComponent

new Watcher(vm, updateComponent)

文件路径：`～/github/vuejs/vue/src/core/instance/lifecycle.js`


## initLifecycle

---

文件路径：`～/github/vuejs/vue/src/core/instance/lifecycle.js`

## updateDOMListeners

createElem
    -> invokeCreateHooks
        -> updateDOMListeners

