---
title: js基本算法
date: 2018-08-27 11:23:54
tags:
- js算法
categories: 前端
---
> 时间复杂度：算法的时间复杂度是一个函数，描述了算法的运行时间。时间复杂度越低，效率越高

> 一个算法，运行了几次时间复杂度就为多少，如运行了n次，则时间复杂度为O(n)

### 1、冒泡排序

> 比较相邻的两个元素，如果前一个比后一个大，则交换位置。
按照相邻元素的比较，第一轮的时候最后一个元素已经为最大的一个，所以最后一个元素不用比较
<!--more-->

``` JavaScript
 function bubbleSort(arr){
    for(var i=0; i<arr.length-1; i++){
        for(var j=0; j<arr.length-i-1; j++){
            //>从小到大排序
            //<从大到小排序
            if(arr[j] > arr[j+1]){
                var temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp
            }
        }
    }
    return arr;
}
var arr=[5,6,2,1,3,8,7,1.2,5.5,4.5];
console.log(bubbleSort(arr));
```

### 2、快速排序

> 快速排序是对冒泡排序的一种改进，第一次排序时将数据分为两部分，一部分比另一部分的所有数据都要小。然后递归调用，在两边都实行快速排序。

``` JavaScript
function quickSort(arr){
    if(arr.length <= 1){return arr;}
    //中心点
    var pivotIndex = Math.floor(arr.length / 2);
    var pivot = arr.splice(pivotIndex, 1)[0];

    var left = [],
          right = [];
    //左右都实行快速排序
    for(var i=0; i<arr.length; i++){
        if(arr[i] < pivot){
            left.push(arr[i]);
        }else{
            right.push(arr[i]);
        }
    }
    //递归 连接左右
    return quickSort(left).concat([pivot], quickSort(right));
}
var arr=[5,6,2,1,3,8,7,1.2,5.5,4.5];
console.log(quickSort(arr));
```

### 3、判断一个单词是否有回文

> 回文是指把相同的词汇或句子，在下文中调换位置或颠倒过来，产生首尾回环的情趣，叫做回文，也叫回环。比如 mamam redivider

``` JavaScript
function checkPalindrom(str) {  
    return str == str.split('').reverse().join('');
}
```