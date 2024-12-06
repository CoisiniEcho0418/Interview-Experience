# Arrays.sort()

------

### 常用默认升序排序

`public static void sort(T[] a)`：

```java
int[] arr = new int[]{2,5,8,9,6,1};
System.out.println("排序前："+Arrays.toString(arr));
Arrays.sort(arr);
System.out.println("排序后："+Arrays.toString(arr));
/* 
输出：
排序前：[2, 5, 8, 9, 6, 1]
排序前：[1, 2, 5, 6, 8, 9]
*/
```



### 部分排序

`public static void sort(T[] a, int fromIndex, int toIndex)`:

```java
int[] arr = new int[]{5,4,3,2,1};
System.out.println("排序前："+Arrays.toString(arr));
Arrays.sort(arr,1,4);
System.out.println("排序后："+Arrays.toString(arr));
/*
排序前：[5, 4, 3, 2, 1]
排序后：[5, 2, 3, 4, 1]
*/
```

> **注意：包括参数中的起始位置，不包括终止位置**



### 自定义排序（可以实现降序）

`public static <T> void sort(T[] a, Comparator<? super T> c)`

- 匿名内部类实现

  ```java
  // 一维数组排序
  Integer[] arr = {1,5,8,4,2,6,4};
  System.out.println("排序前："+Arrays.toString(arr));
  
  Arrays.sort(arr, new Comparator<Integer>()
              {
                  @Override
                  public int compare(Integer o1, Integer o2) {
                      return o2-o1;
                  }
              });
  System.out.println("排序后："+Arrays.toString(arr));
  /*
  排序前：[1, 5, 8, 4, 2, 6, 4]
  排序后：[8, 6, 5, 4, 4, 2, 1]
  */
  ```

  ```java
  // 二维数组排序
  Arrays.sort(intervals, new Comparator<int[]>() {
              public int compare(int[] interval1, int[] interval2) {
                  return interval1[0] - interval2[0];
              }
          });
  ```

  

- 新建一个类然后实现Comparator接口

  ```java
  public class Main{
  	public static void main(String[] args){
  		Integer[] arr = {1,5,8,4,2,6,4};
  		System.out.println("排序前："+Arrays.toString(arr));
  		Comparator myComparator = new MyComparator();
          Arrays.sort(arr, myComparator );
  		System.out.println("排序后："+Arrays.toString(arr));
  	}
  }
  class MyComparator implements Comparator<Integer>{
  
  	@Override
  	public int compare(Integer o1, Integer o2) {
  		return o2-o1;
  	}
  	
  }
  ```

> **注意：Comparator中的类型是Integer，排序的数组也只能是Integer类型，不能够是int类型（可以是int[]）**

