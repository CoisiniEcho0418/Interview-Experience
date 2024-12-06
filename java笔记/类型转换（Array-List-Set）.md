# Java中Array/List/Set之间的转换

------

## Array转List

- 方法一（调用`Arrays.asList()`）：

  ```java
  // 转换后的list操作仍然反映在原数组上，因此这个list是定长的，无法使用list的add()喝remove()方法(无法进行添加删除操作)
  String[] s = new String[]{"A", "B", "C", "D","E"};
  List<String> list = Arrays.asList(s);
  ```

  需要注意用该方法转换后，修改`s`，`list`也会跟着改

- 方法二（调用`Collections.addAll()`）：

  ```java
  // String[] arr
  List<String> res = new ArrayList<>();
  Collections.addAll(res, arr);
  ```

- 方法三：

  ```java
  // String[] arr
  List<String> res = new ArrayList<>();
  for(String str : arr){
  	res.add(str);
  }
  ```

## List转Array

```java
String[] arr = list.toArray(new String[list.size()]);
// 下面这种也是可以的（推荐上面这种）
String[] arr = list.toArray(new String[0]);//new String[0]是指定返回数组的类型
```



## List转Set

```java
// List转Set（注意要对入参判断是否为空，才能使用构造方法）
Set<String> set = new HashSet<>(list);
```

## Set转List

```java
// Set转List（注意要对入参判断是否为空，才能使用构造方法）
List list = new ArrayList<>(set);
```



## Array转Set

- 方法一：

  ```java
  // String[] arr
  Set<String> res = new HashSet<>();
  for(String str : arr){
      res.add(str);
  }
  ```

- 方法二（调用`Collections.addAll()`）：

  ```java
  // String[] arr
  Set<String> res = new HashSet<>();
  Collections.addAll(res, arr);
  ```

- 方法三：

  ```java
  // String[] arr
  Set<String> set = new HashSet<>(Arrays.asList(arr));
  ```

  推荐方法三

## Set转Array

和List转Array类似

```java
String[] arr = set.toArray(new String[set.size()]);
// 下面这种也是可以的（推荐上面这种）
String[] arr = set.toArray(new String[0]);//new String[0]是指定返回数组的类型
```

