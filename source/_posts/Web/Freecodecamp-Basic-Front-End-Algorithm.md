---
title: Freecodecamp 刷题记录——前端基础算法
date: 2018-11-24 11:23:09
tags:
- freecodecamp
description: Freecodecamp Basic Front End Development Projects
---

## [Basic Front End Development Projects](https://www.freecodecamp.cn/map-aside#nested-collapseBasicFrontEndDevelopmentProjects)

### Reverse a String
```
function reverseString(str) {
  // 请把你的代码写在这里
  var array = str.split('');
  array.reverse();
  console.log(array);
  str = array.join('');
  return str;
}

reverseString("hello");
```
### Factorialize a Number
```
function factorialize(num) {
  // 请把你的代码写在这里
  if (num === 0){
    num = 1;
  }
  else{
    for (var i=num-1; i>1; i--){
     num = num * i;
    }
  }
  return num;
}

factorialize(5);
```
### Check for Palindromes
```
function palindrome(str) {
  // 请把你的代码写在这里
  str = str.toLowerCase();
  str = str.replace(/[\W\s_]/g,'');
  
  console.log(str);
  
  n = str.length;
  for (i=0; i<n/2; i++){
    if (str[i] != str[n-i-1]){
       return false;
    }
  }
  return true;
}
palindrome("eye");
```
### Find the Longest Word in a String
```
function findLongestWord(str) {
  // 请把你的代码写在这里
  var array = str.split(' ');
  var length = 0;
  array.forEach(function(word){
    if  (word.length>length) {
      length = word.length;
    }
  });
  return length;
}

findLongestWord("The quick brown fox jumped over the lazy dog");
```
### Title Case a Sentence
```
function titleCase(str) {
  // 请把你的代码写在这里
  var array = str.split(' ');
  array.forEach(function(word, i){
        array[i] = word[0].toUpperCase() + word.substring(1).toLowerCase();
  });
  str = array.join(' ');
  return str;
}

titleCase("I'm a little tea pot");
```
### Return Largest Numbers in Arrays
```
function largestOfFour(arr) {
  // 请把你的代码写在这里
  arr.forEach(function(a, i){
    var m = a[0];
    for (var j=1; j<a.length; j++){
      if (a[j]>m){
        m = a[j];
      }
    }
    arr[i] = m;
  });
  return arr;
}

largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```
### Confirm the Ending
```
function confirmEnding(str, target) {
  // 请把你的代码写在这里
  var pos = str.length - target.length;
    if (pos < 0){
    return false;
  }
  
    if (str.substring(pos) == target){
        return true;
    }
    else{
      return false;

    }
}

confirmEnding("Bastian", "n");
```
### Repeat a string repeat a string 
```
function repeat(str, num) {
  // 请把你的代码写在这里
  var retStr = str;
  if (num < 0){
    retStr = "";
  }
  else{
    for (var i=1; i<num; i++){
      retStr += str;
    }
  }
  return retStr;
}

repeat("abc", 3);
```
### Truncate a string
```
function truncate(str, num) {
  // 请把你的代码写在这里
  if (str.length > num){
    
  
  if (num <= 3){
    str = str.substring(0, num) + "...";
  }
  else{
    str = str.substring(0, num - 3) + "...";
  }
  }
  return str;
}

truncate("A-tisket a-tasket A green and yellow basket", 11);
```
### Chunky Monkey
```
function chunk(arr, size) {
  // 请把你的代码写在这里
  var retArr = [];
  
  for (var i=0; i<arr.length; i+=size){
    retArr.push(arr.slice(i, i+size));
  }
  
  return retArr;
}

chunk(["a", "b", "c", "d"], 2);
```

### Slasher Flick Incomplete
```
function slasher(arr, howMany) {
  // 请把你的代码写在这里
  if (howMany >= 0){
    if(howMany >= arr.length){
      arr = [];
    }
    else{
      return arr.slice(howMany);
    }
  }
  return arr;
}

slasher([1, 2, 3], 2);

```

### Mutations Incomplete
```
function mutation(arr) {
  // 请把你的代码写在这里
  //原生js获取的DOM集合是一个类数组对象，所以不能直接利用数组的方法（例如：forEach，map等），需要进行转换为数组后，才能用数组的方法！
  //https://blog.csdn.net/m0_38082783/article/details/78131036?locationNum=10&fps=1
  var patStr = Array.from(arr[1]);
  var str = Array.from(arr[0]);
  for (var i=0; i<patStr.length; i++){
    var patt = new RegExp(patStr[i], "i");
    if (!patt.test(str)){
      return false;
    }
  }
  return true;
}

mutation(["hello", "hey"]);

```

### Falsy Bouncer Incomplete
```
function bouncer(arr) {
  // 请把你的代码写在这里
  
  return arr.filter(function(word){
    return Boolean(word);
  });
}

bouncer([7, "ate", "", false, 9]);

```

### Seek and Destroy Incomplete
```
function destroyer() {
  var array = arguments[0];
  var datas = [];
for (var i=1; i<arguments.length; i++){
    datas.push(arguments[i]);
}
  return array.filter(function(data){
    for (i=0; i<datas.length; i++){
        console.log("argu:" + datas[i]);
         if (data === datas[i]){
           return false;
         }
    }
    return true;
  });

}

destroyer([1, 2, 3, 1, 2, 3], 2, 3);

```

### Where do I belong Incomplete
```
function where(arr, num) {
  // 请把你的代码写在这里
  var array = Array.from(arr);
  array.sort(function(a, b){
    return a - b;
  });
  for (var i=0; i<array.length; i++){
    if (array[i]>=num){
       return i;
    }
  }
  return array.length;
}

where([40, 60], 50);

```

### Caesars Cipher Incomplete
```
function rot13(str) { // LBH QVQ VG!
  // 请把你的代码写在这里
    var array = Array.from(str);
    array = array.map(function(letter){
        if ('A'<=letter && 'Z'>=letter){
            code = (letter.charCodeAt() - 13);
            if (code < 'A'.charCodeAt()){
                code += 26; 
            }
            letter = String.fromCharCode(code);
            console.log((letter));
        }
        return letter;
    });
  return array.join("");
}


rot13("SERR PBQR PNZC");  // 你可以修改这一行来测试你的代码

```


