---
title:  Stream流常用方法
index_img:  /img/1.jpg
banner_img: /img/1.jpg
categories: java
tags: [笔记]
date:  2021-03-16 16:30:42
---

## [](#前言 "前言")前言
Steam流是JDK8的新特性中的一种,和lambda表达式感觉Stream流的写法不太好记,所以稍微系统一点的捋一下Stream流相关的知识。

## [](#什么是Steam流 "什么是Steam流")什么是Steam流
{% note warning%}
Stream（流）是一个来自数据源的元素队列并支持聚合操作

 元素：是特定类型的对象，形成一个队列。Java中的Stream并不会存储元素，而是按需计算。

 数据源 ：流的来源。可以是集合，数组，I/O channel，产生器generator等。

 聚合操作： 类似SQL语句一样的操作，比如filter, map, reduce, find,match, sorted等。
 {% endnote %} 
 
 上面是比较系统的解释。简单和通俗一点来讲的话,实际使用中,我更多的是将Stream流作为类似foreach和迭代器Iterator的一个加强升级版来使用的。

## [](#常用方法和示例 "常用方法和示例")常用方法和示例

以下简单举例几个Stream流的常用的方法和场景吧
    
- **过滤空字符串**
```
List<String> strings = Arrays.asList("abc", "", "bc", "bcd", "abcd","", "aaa");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```
    
    
- **forEach**
Stream 提供了新的方法 'forEach' 来迭代流中的每个数据。使用 forEach 输出了10个随机数：
```
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```
注："：："就是把方法当做参数传到stream内部，使stream的每个元素都传入到该方法里面执行一下。使用方法-- 类名：：方法名

- **map**
map 方法用于映射每个元素到对应的结果，使用 map 输出了元素对应的平方数：
```
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
// 获取对应的平方数
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```
- **filter**
filter 方法用于通过设置的条件过滤出元素。使用 filter 方法过滤出空字符串：
```
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
long count = strings.stream().filter(string -> string.isEmpty()).count();
```
  
  
- **limit**
limit 方法用于获取指定数量的流。使用 limit 方法打印出 10 条数据：
```
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```

- **sorted**
sorted 方法用于对流进行排序。使用 sorted 方法对输出的 10 个随机数进行排序：
```
Random random = new Random();
random.ints().limit(10).sorted().forEach(System.out::println);
```
  
  
- **Collectors**
Collectors 类,将流转换成集合和聚合元素。Collectors 可用于返回列表或字符串：
```
//将流转换为List(基本方法)
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
  
  
//将流转换为Map
List<Person> list = new ArrayList();  
        list.add(new Person("1001", "小A"));  
        list.add(new Person("1002", "小B"));  
        list.add(new Person("1003", "小C"));
Map<String, String> map = list.stream().collect(Collectors.toMap(Person::getId, Person::getName));
```

- **其他**
另外还有一些比如说统计基本类型的一些方法,比如：
```
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
 
IntSummaryStatistics stats = numbers.stream().mapToInt((x) -> x).summaryStatistics();
 
System.out.println("列表中最大的数 : " + stats.getMax());
System.out.println("列表中最小的数 : " + stats.getMin());
System.out.println("所有数之和 : " + stats.getSum());
System.out.println("平均数 : " + stats.getAverage());
```