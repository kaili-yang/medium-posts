
# Top Array interview questions you must know
## Basic interview questions
### 1. Which of the data structures are linear table data structures?
Arrays, Linked Lists, Stacks, Queues.

### 2. What is the time complexity of deleting and obtaining elements of an array?
The time complexity of obtaining array elements is o(1), because the memory address can be accurately obtained according to the index, and deletion, in the worst case, is o(n). Assuming that the first element is deleted, it is the first step For 1, the remaining (n-1) elements need to be moved forward one bit at a time, so it is 1+(n-1)=n

### 3. What is the default initialized length in ArrayList?
The default initial length is 10

### 4. What is the range of expansion in ArrayList?
Every time an element is stored, it will be judged whether the maximum capacity is exceeded, and if it is exceeded, the capacity will be expanded by 1.5 times the circle capacity.

### 5. How does ArrayList complete expansion, and what are the functions of each input parameter of System.arraycopy?
The key method of expansion is grow(). When adding elements, it is judged whether the maximum capacity is exceeded. If it is exceeded, the capacity is expanded. First, a new array is created and the old data is copied to the new array, so that there is a copy. Next The operation will not affect the original data, and then expand the capacity to 1.5 times the original size, and finally call the Arrays.copyOf method to point the elementData array to the continuous space of newCapacity in the new memory space.

The role of each input parameter of `System.arraycopy` is simply to copy a piece of data in the array of the first parameter to the second array.

## I. Introduction
An array is just a name, it can describe a set of operations, and it can also name the set of operations. The data operation of the array is handled by idx->val. It does not specifically require that continuous data be stored in the memory to be called an array, but that adjacent data can also be accessed linearly through continuous index idx.

Then when you define how the data is stored, you also define the data structure. So it is also classified as a data structure.

## II. Array data structure
Array is a linear table data structure. It uses a set of contiguous memory spaces to store a set of data of the same type.

Features of arrays:
1. An array is a collection of elements of the same data type (int cannot store double)
2. The storage of each element in the array is sequential, and they are continuously stored together in this order in the memory. The memory addresses are contiguous.
3. The time complexity of getting elements from an array is O(1)

### 1. One-dimensional array
One-dimensional arrays are the most commonly used arrays, and many other variants of data structures are derived from one-dimensional arrays. For example, the zipper addressing structure of HashMap and the open addressing structure of ThreadLocal are all implemented from one-dimensional arrays.

### 2. Two-dimensional array
Two-dimensional and multi-dimensional arrays are rarely used in development scenarios, but they can be used in some algorithmic logic and mathematical calculations.

## III. Initialize an array list
In Java source code, array is a very commonly used data structure, and many other data structures also have the shadow of array. In some data storage and use scenarios, ArrayList is basically used instead of LinkedList. For specific performance analysis, refer to: Is LinkedList faster to insert than ArrayList? are you sure? 

Then in this chapter, we will implement a simple ArrayList by learning the array structure, so that readers who use Java can understand both the data structure and the Java source code implementation.

### 1. Basic design
An array is a fixed, continuous, and linear data structure, so if you want to use it as an array list that automatically expands its capacity, you need to do some expansion.

```
/**
 * default initial space
 */
private static final int DEFAULT_CAPACITY = 10;
/**
 * null element
 */
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
/**
 * ArrayList array buffer
 */
transient Object[] elementData;
```

1. In the stage of initializing the ArrayList, if the size is not specified, an empty element will be initialized by default. At this time, there is no default length.
2. So when to give the initialized length? It is when adding elements for the first time, because all operations of adding elements also need to judge the capacity and whether to expand. Then it is easier to handle this matter uniformly when adding elements.
3. After that, with the addition of elements, the capacity will be insufficient. When the capacity is insufficient, capacity expansion is required. At the same time, it is necessary to migrate the old data to the new array. Therefore, data migration is a relatively time-consuming operation.

### 2. Add elements
```
public boolean add(E e) {
    // Ensure internal capacity
    int minCapacity = size + 1;
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    // Determining expansion operations
    if (minCapacity - elementData.length > 0) {
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0) {
            newCapacity = minCapacity;
        }
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    // Adding element
    elementData[size++] = e;
    return true;
}
```
This is a simplified ArrayList#add operation
1. To judge the current capacity and the initial capacity, use the Math.max function to take the maximum value as the minimum initial space.
2. The next step is to judge whether minCapacity and the number of elements have reached expansion. The first time ArrayList is created, it will definitely be expanded, that is, the capacity of DEFAULT_CAPACITY = 10 will be initialized.
3. Arrays.copyOf actually creates a new spatial array, and then calls System.arraycopy to migrate to the newly created array. In this way, all subsequent expansion operations will remain unified.
4. After the expansion of ArrayList is completed, use elementData[size++] = e; to add elements.

### 3. Remove elements
The focus of ArrayList is inseparable from the use of System.arraycopy, which is a native method that allows you to migrate from a specific position in the original array to a specified position and migration amount in the new array. As shown in Figure 2–5, the data migration test code is in java-algorithms
```
public E remove(int index) {
    E oldValue = (E) elementData[index];
    int numMoved = size - index - 1;
    if (numMoved > 0) {
        // Copy n elements from a position in the original array to a position in the target object
        System.arraycopy(elementData, index + 1, elementData, index, numMoved);
    }
    elementData[--size] = null; // clear to let GC do its work
    return oldValue;
}
```
1. The element deletion of ArrayList is to use System.arraycopy to copy the data to move the data after determining the position of the element, and overwrite the position of the element to be deleted.
2. In addition, it will also set the deleted element to null. On the one hand, we will not read this element again, and on the other hand, it is also for GC

### 4. Get elements
```
public E get(int index) {
    return (E) elementData[index];
}
@Override
public String toString() {
    return "ArrayList{" +
            "elementData=" + Arrays.toString(elementData) +
            ", size=" + size +
            '}';
}
```

It is relatively simple to get the element, just use the index directly from elementData to get it. This is an O(1) operation. It is also because of the convenience of searching for elements that ArrayList is so widely used. At the same time, in order to be compatible, data can be obtained through elements instead of directly through subscripts, which leads to the calculation method of HashMap using hash value to calculate subscripts, and also leads to Fibonacci hashing. They are all designed to obtain data with a time complexity as close to O(1) as possible while reducing element collisions as much as possible. To learn these contents, you can read Brother Fu's "Java Face Manual" or learn after gradually covering the algorithm with the content of this series of chapters.

### 4. Array list test
```
@Test
public void test_array_list() {
    cn.bugstack.algorithms.data.array.List<String> list = new ArrayList<>();
    list.add("01");
    list.add("02");
    list.add("03");
    list.add("04");
    list.add("05");
    list.add("06");
    list.add("07");
    list.add("08");
    list.add("09");
    list.add("10");
    list.add("11");
    list.add("12");
    
    System.out.println(list);
    
    list.remove(9);
    
    System.out.println(list);
}
```
Test results
```
ArrayList{elementData=[01, 02, 03, 04, 05, 06, 07, 08, 09, 10, 11, 12, null, null, null], size=12}
ArrayList{elementData=[01, 02, 03, 04, 05, 06, 07, 08, 09, 11, 12, null, null, null, null], size=11}

Process finished with exit code 0
```

- The test case includes sequentially adding elements in our own implemented ArrayList, gradually testing the expansion and migration of elements, and data migration after deleting elements.
- As can be seen from the final test results, there are a total of 12 elements, and the migration of elements changes before and after the element with idx=9 is deleted.
