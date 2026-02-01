---
title: ArrayList Review
tags:
  - java
  - 回顾
date: 2025-01-19 16:48:02
description: 对ArrayList集合的使用的回顾，一些基础操作
categories:
password:
message:
cover: https://img.znxs.vip/study/java-arraylist-1.png
top_img: https://img.znxs.vip/study/java-arraylist-1.png
---

# ArrayList Review



ArrayList 是一个数组集合列表，是最常用的动态数组的实现，提供了许多方法，以便快速访问和遍历场景



## Init（新建ArrayList集合）

```java
List<String> arrayList = new ArrayList<>();
```



## Add（添加元素）

1. **add (E)** 	添加指定元素
   **add (int, E)** 	指定位置添加指定元素

   ```java
   // 通过 add 方法增添新元素 这里使用了随机值转string 的方式
   arrayList.add(String.valueOf((Math.random()*10)));
   ```

   这里的add方法有几个重载 （其实是对AbstractList的重写）

   - `boolean add(E)` 

     这个方法接受泛型参数，接受任意参数

   - `void add(int, E)`

     这个方法接受一个整数 和 一个泛型参数，这个整数值可以用于在指定位置插入参数值

     ```java
     // 在 第二个位置插入 ，其他位置往后挪一位
     arrayList.add(1,String.valueOf((Math.random()*10)));
     ```

2. **addAll (Collection<? extends E>)**    添加指定集合

   ```java
   // addAll方法，将指定集合添加到该集合的尾部
   List<String> newString = Arrays.asList("123", "456");
   arrayList.addAll(newString);
   ```

   同样，也可以使用重载的int参数指定添加位置

   ```java
   arrayList.addAll(2,newString);
   ```



## Remove（删除元素）

1. **remove(E)**    	从集合中删除指定元素

   ```java
   // 删除指定元素
   arrayList.remove("123");
   ```

2. **remove(int)**        从集合指定位置删除元素

   ```java
   // 从指定位置删除元素
   arrayList.remove(1);
   ```

3. **removeAll(Collection<? extends E>)**    从集合中删除指定集合中的所有元素 

   ```java
   // 从集合中删除指定集合中的所有元素 
   List<String> newString = Arrays.asList("123", "456");
   arrayList.removeAll(newString);
   ```

4. **clear()**    清空集合

   ```java
   arrayList.clear();  // 列表变为空数组[] 
   ```



## Get（获取元素）

1. **get(int)**    获取指定位置的元素

   ```java
   // 获取指定位置的元素
   E result = arrayList.get(1);
   ```



## Set（修改元素）

1. **set(int, E)**    修改指定位置的元素为指定元素

   ```java
   //  修改指定位置的元素为指定元素
   arrayList.set(3,"3");
   ```



## Index（查找元素）

1. **indexOf(E)**   返回指定元素**第一次**出现的位置，如果不存在，返回-1

   ```java
   int index = arrayList.indexOf("123"):
   ```

2. **lastIndexOf(E)**    返回指定元素**最后一次**出现的位置，如果不存在，返回-1

   ```java
   int index = arrayList.lastIndexOf("123"):
   ```

3. **contains(E)**    判断集合是否包含指定元素

   ```java
   boolean isContains = arrayList.contains("123"): 
   ```



## Other（其他方法）

1. **size()**   返回集合元素数量

   ```java
   arrayList.size();
   ```

2. **isEmpty()**    判断集合是否为空

   ```java
   arrayList.isEmpty();
   ```

3. **toArray()**    将列表转换成数组

   ```java
   arrayList.toArray();   // 返回结果是数组
   ```

4. **subList(int, int)**    返回指定范围 [n,m )**左闭右开**的子列表

   ```java
   arrayList.subList(2,9);
   ```



## For （遍历列表）

1. 使用for循环遍历

   ```java
   for(int i = 0 ;i<arrayList.size(); i++){
       System.out.println(arrayList.get(i));
   }
   ```

2. 使用加强for循环

   ```java
   for(E item : arrayList){
       System.out.println(item);
   }
   ```

3. 使用迭代器

   ```java
   Iterator<E> iterator = arrayList.iterator();
   while(iterator.hasNext()){
       System.out.println(iterator.next());
   }
   ```

4. 使用lambda表达式 forEach方法

   ```java
   arrayList.forEach(item->System.out.println(item));
   ```



## Sort （排序）

1. **sort(Collection<? super E>)**   根据**指定的比较器**对集合进行排序

   ```java
   arrayList.sort(Comparator.naturalOrder());  // 自然排序
   ```



## All（批量操作）

1. **replaceAll(UnaryOperator< E >)**    对集合的每个元素进行指定操作

   ```java
   arrayList.replaceAll(item->item.toUpperCase());
   ```

2. **retainAll(Collection<?>)**      仅保留列表中包含在内的指定列表中的元素

   ```java
   List<String> newString = Arrays.asList("123", "456");
   arrayList.retainAll(newString);  // 仅保留arrayList中的newString列表的内容
   ```

   
