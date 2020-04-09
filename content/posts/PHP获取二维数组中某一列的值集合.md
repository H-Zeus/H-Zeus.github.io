---
title: PHP获取二维数组中某一列的值集合
tags: ["PHP"]
categories: ["PHP"]
date: 2020-04-08
---

PHP还是比较常用的，于是我研究了一下PHP二维数组。在处理php数组的时候，有一种需求特别的频繁，如下二维数组：

```php
$arr = array(
    1=>array(
        'id' => 5,
        'name' => '张三'
    ),
    2=>array(
    'id' => 6,
    'name' => '李四'
    )
);
```

目的就是要取到key为name的集合，得到这个结果：

```php
$arr2 = array(
    0=>'张三',
    1=>'李四'
);
```

这里有几种方法：
1：最简单的，foreach遍历数组：
```php
foreach ($arr as $key => $value) {
    $arr2[] = $value['name'];
}
```

2：代码量稍微少一点的代码，使用了 array_map 这个php方法：
```php
$arr2 = array_map('array_shift',$arr);
```
意为把$arr数组的每一项值的开头的值移出，并返回被移出的每一项值中被移出的值，注意此时新数组$arr2的键仍是原数组$arr的键

2.1：在方法2的基础上，可以稍微开一下脑洞，如果需要获取的是二维数组的每一项的开头列或结尾列，也可以这样做：
```php
$arr2 = array_map('reset',$arr);
$arr2 = array_map('end',$arr);
```    

3：还可以使用 array_reduce 方法，不过代码略多，不过这个方法的想象空间（针对别的数组取值操作）还是蛮大的：
```php
$arr2 = array_reduce($arr, create_function('$result, $v', '$result[] = $v["name"];return $result;'));
```
array_reduce方法用回调函数迭代地将对数组的值进行操作，而create_function用于一个匿名方法做回调用，这个匿名方法的参数$result为上一次迭代产生的值，$v是当前的值，内部实现既为获取到数组$arr每一项的”name”的值并push到新$result数组;

4：最后这个终极方法实在是太爽了，一个方法就搞定，而且非常灵活：
```php
$arr2 = array_column($arr, 'name');
```
第二个参数为想要获取的那一列的键名，是不是很方便呢，不过这个方法有个限制，就是php版本必须 >= 5.5.0，在陈旧项目中使用这个方法还是得斟酌一下哈
