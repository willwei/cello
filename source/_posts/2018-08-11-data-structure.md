---
title: 数据结构笔记
date: 2018-08-11
tags: 
- data-structure
---

## shuffle

Fisher-Yates

```javascript
var source_array = [1,2,3,4,5]

function shuffle(source_array) {
  var target_array = []
  while(source_array.length) {
    var random_index = Math.floor(Math.random() * source_array.length)
    target_array.push(source_array[random_index])
    source_array.splice(random_index, 1)
    console.log(random_index, source_array)
  }
  return target_array
}

var result = shuffle(source_array)

console.log(result)
```

## 两个有序数组合并

```javascript
var arr1 = [2, 7, 15, 23]
var arr2 = [8, 9, 10, 11]

function sort_2_ordered_array(arr1, arr2) {
  var result = [];
  var index1 = 0, index2 = 0, len1 = arr1.length, len2 = arr2.length
  while (index1 < len1 && index2 < len2) {
      if (arr1[index1] < arr2[index2]) {
        result.push(arr1[index1])
        index1 ++
      } else {
        result.push(arr2[index2])
        index2 ++
      }
  }
  
  while (index1 < len1) {
    result.push(arr1[index1])
    index1 ++
  }
  
  while (index2 < len2) {
    result.push(arr2[index2])
    index2 ++
  }
  
  return result
}

var result = sort_2_ordered_array(arr1, arr2)

console.log(result)
```


## 获取一个数组中第二大的值


## 求解一颗二叉树的最大权重路径


## 数组去重
