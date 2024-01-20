---
title: JavaScript-数组对象
tags: JavaScript
categories: JavaScript
img: /image/6.jpg
cover: false
summary: 第八章：JavaScript数组对象
abbrlink: 357e
date: 2020-08-23 21:09:05
---

# JavaScript学习笔记



## 第八章 JavaScript数组对象

### 1、数组对象

数组：使用单独的变量名来储存一系列的值，数组可以存储任意数据类型的数据，具有长度和下标。

### 2、创建数组

- 字面量方式创建

  ```js
  var arr = ['ahb',123,function(){},{},null,undefined,[1,'sd']];
  ```

- 构造函数创建

  ```js
  var arr1 = new Array();//[]空数组
  var arr2 = new Array(1,2,3,4);//[1,2,3,4]
  //注意：如果参数只有一个值且是数值，代表创建一个该长度的空数组
  var arr3 = new Array(4);//[empty × 4]长度4的空数组
  ```

### 3、遍历数组

- 使用for循环遍历配合下标和长度遍历

  ```js
  var arr4 = [1,2,3,4,'a','b','c'];
  for(var i = 0;i < arr4.length;i++){
  	console.log(arr4[i]);
  }
  //1 2 3 4 "a" "b" "c"
  ```

- for-in()，遍历对象的标配方法，遍历数组时返回的数组下标是字符串，所以不常用，使用时需注意。

  ```js
  var arr4 = [1,2,3,4,'a','b','c'];
  for(var key in arr4){
  	console.log(key,typeof key,arr4);
  }
  /*
  0 string (7) [1, 2, 3, 4, "a", "b", "c"]
  1 string (7) [1, 2, 3, 4, "a", "b", "c"]
  2 string (7) [1, 2, 3, 4, "a", "b", "c"]
  3 string (7) [1, 2, 3, 4, "a", "b", "c"]
  4 string (7) [1, 2, 3, 4, "a", "b", "c"]
  5 string (7) [1, 2, 3, 4, "a", "b", "c"]
  6 string (7) [1, 2, 3, 4, "a", "b", "c"]
  */
  ```


4、数组的操作方法

- 栈方法：在数组末尾操作元素，可以改变原数组。

  - push()：在数组末尾添加任意个数的元素，**改变原数组，返回新数组长度**。

    ```js
    var arr = ['a','b','c'];
    console.log(arr.push('d','e'));//5
    console.log(arr);//(5) ["a", "b", "c", "d", "e"]
    ```

  - pop()：从数组末尾移除最后一项，没有参数，不能指定删除元素，**改变原数组，返回的是被删除的元素**，一次只能删除一个。

    ```js
    var arr1 = ['ab','cd','ef','jh'];
    console.log(arr1.pop());//jh
    console.log(arr1);//(3) ["ab", "cd", "ef"]
    ```

- 队列方法：在数组头部操作元素，可以改变原数组。

  - unshift()：在数组前端添加任意个数的元素，**改变原数组，并返回新数组的长度**。

    ```js
    var arr3 = ['a','b','c'];
    console.log(arr3.unshift('d','e'));//5
    console.log(arr3);//(5) ["d", "e", "a", "b", "c"]
    ```

  - shift()：从数组前端移除第一项，没有参数，不能指定删除元素，**改变原数组，返回的是被删除的元素**，一次只能删除一个。

    ```js
    var arr4 = ['ab','cd','ef','jh'];
    console.log(arr4.shift());//ab
    console.log(arr4);//(3) ["cd", "ef", "jh"]
    ```

- splice()：删除、添加、替换

  在当前数组中删除一段连续的元素，并且可以在被删除的位置上添加新的元素，**改变原数组**，**以数组的形式返回被删除的元素**。

  splice(start,deleteCount,item1,…,itemX)

  - 删除

    splice(起始位置，删除项数)，从起始下标位置开始删除包括该位置，删除项数是Number类型，如果删除项数省略则默认删到末尾。

    ```js
    var arr5 = ['a','b','c','d'];
    console.log(arr5.splice(2,2));//(2) ["c", "d"]
    console.log(arr5);//(2) ["a", "b"]
    ```

  - 添加

    splice(添加位置所在下标，0，’添加元素1‘，…，’添加元素n‘)，添加位置所在下标会将原来该位置的元素往后移。第二个参数为0，表示不删除，第二个参数之后可以是多个被添加的参数。返回值为空值，因为没有删除元素。

    ```js
    var arr6 = ['a','b','c','d'];
    console.log(arr6.splice(2,0,'e','f'));//[]
    console.log(arr6);//(6) ["a", "b", "e", "f", "c", "d"]
    ```

  - 替换

    splice(起始位置，删除项数，’替换元素1‘，…，’替换元素n‘)，从起始下标位置开始替换包括该位置，删除项数是Number类型，第二个参数之后可以是多个替换的参数。

    ```js
    var arr7 = ['a','b','c','d','e'];
    console.log(arr7.splice(2,2,'f','g'));//(2) ["c", "d"]
    console.log(arr7);//(5) ["a", "b", "f", "g", "e"]
    //如果第二个参数省略，则会将后面第一个替换的字符串当作删除的字符串，如果查询不到该字符串则不删除，返回空值，将第二个及后面替换的字符插在相应位置。
    console.log(arr7.splice(2,'f','g'));//[]
    console.log(arr7);//(6) ["a", "b", "g", "c", "d", "e"]
    ```

### 4、数组去重

- splice()方法

  ```js
  var arr = [1,3,1,4,7,5,9,4,3,3,1,2];
  for(var i = 0;i<arr.length;i++){
      for(var j= i+1;j<arr.length;j++){
          if(arr[i] == arr[j]){
              arr.splice(j,1);
              j--;//删除元素后，字符串长度减少，j要相应减少
          }
      }
  }
  console.log(arr);//(7) [1, 3, 4, 7, 5, 9, 2]
  ```

- indexOf()、push()方法

  ```js
  var arr = [1,3,1,4,7,5,9,4,3,3,1,2];
  var arr1 = [];
  for(var i=0;i<arr.length;i++){
      if(arr1.indexOf(arr[i]) == -1){
          arr1.push(arr[i]);
      }
  }
  console.log(arr1);//(7) [1, 3, 4, 7, 5, 9, 2]
  ```


### 5、数组排序

- sort()

  该方法按升序排列数组项，只能排序10以下，不包括10，超过10只比较十位数上的大小，**该方法是在原数组上排序会改变原数组顺序**。

  ```js
  var arr = [3,6,2,9,10,7,21,15];
  console.log(arr.sort());//(8) [10, 15, 2, 21, 3, 6, 7, 9]
  ```

  该方法可以接收一个匿名函数作为参数，该函数称为比较函数。

  ```js
  var arr = [3,6,2,9,10,7,21,15];
  arr.sort(function (a,b){
  	//return a-b; //从小到大
      return b-a; //从大到小
  });
  console.log(arr);//(8) [21, 15, 10, 9, 7, 6, 3, 2]
  ```

  用sort()方法对中文进行排序。

  ```js
  var arr1 = [
      {name:'刘备',num:12},
      {name:'孙尚香',num:25},
      {name:'小乔',num:21},
      {name:'孙策',num:15},
      {name:'大乔',num:19}
  ];
  console.log(arr1.sort(function(a,b){
  	return a.name.localeCompare(b.name,'zh');
  }));
  //localeCompare:是把中文转成拼音并拿到第一个字的首字母
  //"zh":以中文的方式排序，按abcd...顺序排序。如果是英文则以Unicode编码进行排序。
  ```

  {% asset_img 8-1.png %}

- 排序算法

  - 选择排序

    从第一项起，每一项都和后面所有项依次进行比较，如果后面的数小于前面的数，则交换位置。

    ```js
    var arr = [12,3,5,1,9,21,5,17];
    for(var i = 0;i<arr.length;i++){
        for(var j = i+1;j<arr.length;j++){
    		if(arr[i] > arr[j]){
                var temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
    		}
        }
    }
    console.log(arr);//(8) [1, 3, 5, 5, 9, 12, 17, 21]
    ```

  - 冒泡排序

    从第一项起，比较相邻的元素，如果前一个比后一个大，则交换位置，最后一个数最大，之后从一个开始重新循环。

    ```js
    var arr = [12,3,5,1,9,21,5,17];
    for(var i = 0;i<arr.length;i++){
        for(var j = 0;j<arr.length;j++){
            if(arr[j] > arr[j+1]){
                var temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
    		}
        }
    }
    console.log(arr);//(8) [1, 3, 5, 5, 9, 12, 17, 21]
    ```

  - 快速排序

    找到数组的中间项，把中间项用splice删除，然后循环数组，如果比这项小的，放在一个left数组中，如果比这项大的，放在一个right数组中，然后递归调用快速排序方法。

    ```js
    var arr = [12,3,5,1,9,21,5,17];
    function quickSort(array){
        //终止条件
        if(array.length <= 1){
            return array;
        }
        var midIndex = Math.floor(array.length / 2);//中间值下标，向下取整
        var midNum = array.splice(midIndex,1)[0];//中间值
        console.log(midNum)
        var left = [];//左边数组存放小于midNum的值
        var right = [];//右边数组存放大于midNum的值
    
        for(var i=0;i<array.length;i++){
            if(array[i] < midNum){
                left.push(array[i]);//小于中间值存放在左边数组
            }else{
                right.push(array[i]);//大于中间值放在右边数组
            }
        }
        return quickSort(left).concat(midNum,quickSort(right));//左右两边数组递归调用本函数循环比较，最终cancat函数进行字符串连接。
    }
    console.log(quickSort(arr));
    ```


### 6、数组的其他方法

- 数组.join()

  将数组以特定格式拼接成字符串，该方法只接收一个参数，即用作分隔符的字符串，然后**返回生成的字符串，不改变原数组**。

  ```js
  var arr = [1,2,'a','b'];
  console.log(arr.join('-'));//1-2-a-b
  ```

- 数组1.concat(数组2，数组3，…)

  连接数组，该方法的参数接收一个或多个数组（或任意的值），该方法会将这些数组中的每一项都添加到结果数组中。**不改变原数组，返回新数组。**

  ```js
  var arr1 = [1,2,3];
  var arr2 = ['a','d','c'];
  var newArr = arr1.concat(666,'new',arr2);
  console.log(newArr);//(8) [1, 2, 3, 666, "new", "a", "d", "c"]
  ```

- 数组.reverse()

  该方法会反转数组的顺序，**改变原数组并返回**。

  ```js
  var arr = [1,2,3,4,5];
  console.log(arr.reverse());//(5) [5, 4, 3, 2, 1]
  ```

- 数组.indexOf()

  该方法接收两个参数：要查找的项和查找起点位置的索引，查找无果返回-1。

  lastIndexOf()为向后往前找，这两种方法在数组中使用与在字符串中使用情况相同，详细参考第七章字符串对象。

  ```js
  var arr = [1,2,3,4,5,6];
  console.log(arr.indexOf(3));//2
  console.log(arr.indexOf(2,3));//-1，从下标为3的位置开始找2
  ```

- slice()

  截取数组：数组.slice(起始下标，结束下标)，不包括结束下标，**不影响原数组，返回新数组**。当两个参数都省略时，相当于复制数组。

  ```js
  var arr = [1,2,3,4,5,6,7];
  console.log(arr.slice(2,5));//(3) [3, 4, 5]
  console.log(arr);//(7) [1, 2, 3, 4, 5, 6, 7]
  ```

- Array.isArray(参数)

  该方法是一个静态方法（ 静态方法指在构造函数本身上定义的方法，只能通过构造函数本身调用，new出来的对象不能够调用。 ），用来判断是否是一个数组，该方法IE8及以下不支持。

  ```js
  console.log(Array.isArray([]));//true
  console.log(Array.isArray('a'));//false
  ```

### 7、数组的迭代方法

数组迭代，通俗来讲就是数组遍历，js常用的迭代方法有如下：

- forEach(function(value,index){})

  简单的循环数组，没有返回值，函数参数分别是数组的项和下标。

  ```js
  var arr = [1,2,3,4,5];
  arr.forEach(function(value,index){
      console.log(index,value);
  });
  /*
  0 1
  1 2
  2 3
  3 4
  4 5
  */
  ```

- every(function(value,index,array){})

  针对数组元素元素做些判断，如果结果都为true，则返回的结果为true。函数参数分别是数组项、下标、数组本身。

  ```js
  var arr = [1,2,3,4,5];
  var compare= arr.every(function(value){
  	return value > 0;
  });
  console.log(compare);//true
  ```

- some(function(value,index,array){})

  针对数组元素做某些判断，如果有一个为true，则返回的结果为true，且不再比较后面的值。

  ```js
  var arr = [1,2,3,4,5];
  var compare= arr.some(function(value){
  	return value > 4;
  });
  console.log(compare);//true
  ```

- filter(function(value,index,array){})

  针对数组元素做某些判断，满足条件的元素会生成一个新的数组并返回。

  ```js
  var arr = [1,2,3,4,5];
  var compare= arr.fliter(function(value){
  	return value > 4;
  });
  console.log(compare);//(2) [4, 5]
  ```

  