## Java，ArrayList为什么是不同步的？Java中如何实现ArrayList?

因为执行add, delete时，自动更新size的大小。当size = capacity时，将当前数组赋值到一个扩容两倍大小的新数组

```java
int newSize = elements.length * 2;
elements = Arrays.copyOf(elements, newSize);
```
验证java内置ArrayList

```java
    /**
     * Test list, when list size was changed in a loop
     */
    public static void main(String[] args) {

        int [] arr1 = {1,2,3,4,5};
        List<Integer> list1 = new ArrayList<>();
        for (int val : arr1) {
            list1.add(val);
        }


        for (int i = 0; i < list1.size();i++) {
            System.out.println("size:" + list1.size()); // 5 5 5 6 6 6
            if (i == 2)
                list1.add(i);

            //System.out.println("size:" + list1.size()); // 5 5 6 6 6 6

        }
    }

```

### 模拟实现
```java
public class MyList<E> {
    private int size = 0;
    private static final int DEFAULT_CAPACITY = 10;
    private Object elements[];

    public MyList() {
        elements = new Object[DEFAULT_CAPACITY];
    }

    public void add(E e) {
        if (size == elements.length) {
            ensureCapa();
        }
        elements[size++] = e;
    }


    private void ensureCapa() {
        int newSize = elements.length * 2;
        elements = Arrays.copyOf(elements, newSize);
    }

    @SuppressWarnings("unchecked")
    public E get(int i) {
        if (i>= size || i <0) {
            throw new IndexOutOfBoundsException("Index: " + i + ", Size " + i );
        }
        return (E) elements[i];
    }
}
```
