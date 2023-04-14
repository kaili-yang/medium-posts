# Data structure 1. linked list

This is a new collection about knowledge of data structures and related interview questions.

[Data structure 1. linked list](https://medium.com/@124ykl/data-structure-1-linked-list-d67a40e1a909)

<a href="https://www.buymeacoffee.com/kailiyang1X" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 40px !important;width: 145px !important;" ></a>
or give me a star 🌟

-----


In computer science, a linked list is a linear collection of data elements whose linear order is not given by their physical address in memory. It is a data structure consisting of a set of nodes, each element pointing to the next element, which together represent a linear sequence.

In the simplest linked list structure, each node is composed of data and a pointer (a pointer to the next node). This data structure allows for efficient insertion or deletion of elements from any position in the sequence during iteration.

The data structure of the linked list provides more efficient operations of inserting and deleting elements without expanding the space through the connection method of the chain. It is a very convenient data structure in suitable scenarios. However, in some cases that require traversal, specified location operations, or access to arbitrary elements, loop traversal is required, which will lead to an increase in time complexity.

## I. linked list classification type
The main manifestations of linked lists are divided into; one-way linked list, doubly linked list, circular linked list, and then we will introduce them separately.

### 1.Singly linked list
A singly linked list contains nodes with a data field and a "next" field pointing to the next node in the node's row. Operations that can be performed on a singly linked list include insertion, deletion, and traversal.

### 2. Doubly linked list
In a "doubly linked list", in addition to the next node link, each node also contains a second link field pointing to the "previous" node in the sequence. The two links could be called 'forward ('s') and 'backwards', or 'next' and 'prev' ('previous').
### 3. Circular linked list
In the last node of the list, the link field usually contains a null reference, a special value used to indicate the absence of further nodes. A less common convention is to have it point to the first node of the list. In this case, the list is said to be "circular" or "circularly linked"; otherwise, it is said to be "open" or "linear". It is a list where the last pointer points to the first node.
## III. Implement a linked list
The source code of Java itself provides a lot of data structures, including the linked list LinkedList we have learned, which is also very frequent in daily development and use.
Therefore, in the process of learning, we use the language commonly used by Java programmers to analyze and learn, and implement LinkedList by handwriting in a simplified way, so that readers can understand the linked list more conveniently.
### 1. Linked list node
```
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
    
    public Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```
- The core foundation of the data structure of the linked list lies in the use of the node object, and associates the previous and next nodes of the current node in the node object. In this way, a linked list structure is constructed.
- But also because a new Node node needs to be created when adding each element to the linked list, so this is also a time-consuming operation.

### 2. Head Insertion
```
void linkFirst(E e) {
    final Node<E> f = first;
    final Node<E> newNode = new Node<>(null, e, f);
    first = newNode;
    if (f == null)
        last = newNode;
    else
        f.prev = newNode;
    size++;
}
```
- For the operation process of head insertion, first record the head node. Then create a new node, and the head node input parameter of the new node constructor is null, and a new head node is constructed in this way.
- The original head node, set f.prev to connect to the new head node, so that the operation of head insertion can be completed. In addition, if there is no head node, just set the head node to a new node. Finally, record the number of nodes in the current linked list, that is, you get it from this value when you use LinkedList to get the size.

### 3. Tail plug node
```
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null) {
        first = newNode;
    } else {
        l.next = newNode;
    }
    size++;
}
```

- The tail difference node is just the opposite of the head insertion node. A new node is created by recording the current end node, and the current end node is associated with the newly created node through l.next. At the same time, record the value of the number of size nodes.

[Click to Try](https://visualgo.net/en/list)

### 4. Chain removal operation
```
E unlink(Node<E> x) {
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;
    if (prev == null) {
        first = next;
    } else {
        prev.next = next;
        x.prev = null;
    }
    if (next == null) {
        last = prev;
    } else {
        next.prev = prev;
        x.next = null;
    }
    x.item = null;
    size--;
    return element;
}
```
- unlink is an unlinking operation. As long as you give an element, it can connect the previous node of the current element with a node, and then remove itself.
- This method is often used in the operation of remove to remove elements, because the entire operation process does not need to traverse, and there is no need to copy new space after removing elements, so the time complexity is O(1).

### 5. Delete node
```
public boolean remove(Object o) {
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);
                return true;
            }
        }
    }
    return false;
}
```
- The process of deleting elements requires a for loop to judge the value of the deleted element, find the corresponding element, and delete it.
- The process of circular comparison is an O(n) operation, and the process of deletion is an O(1) operation. So if the linked list is large and the deleted elements are all close to the end, then this cycle comparison process is also time-consuming.

### 6. Linked list usage test
```
public static void main(String[] args) {
    List<String> list = new LinkedList<>();
    // 添加元素
    list.add("a");
    list.addFirst("b");
    list.addLast("c");
    // 打印列表
    list.printLinkList();
    // 头插元素
    list.addFirst("d");
    // 删除元素
    list.remove("b");
    // 打印列表
    list.printLinkList();
}
```
Test Results
```
Current list, head node: b, tail node: c, overall: b, a, c
Current list, head node: d, tail node: c Overall: d, a, c.
```
## Common interview questions
1. Describe the data structure of the linked list?
3. Does LinkedList in Java use a one-way linked list, a doubly linked list or a circular linked list?
4. What is the time complexity of inserting, deleting, and obtaining elements in the linked list?
5. In what scenarios is it more appropriate to use a linked list?

Think about them and try to figure out by yourself.
Check your answers.
1. A linked list is a linear collection of data elements whose linear order is not given by their physical address in memory. It is a data structure consisting of a set of nodes, each element pointing to the next element, which together represent a linear sequence.
2. Java uses a circular doubly linked list data structure.
3. For insertion and deletion, if there is a preceding node, the time complexity is 0(1), and if there is no case, it is o(n). To obtain elements needs to be traversed, so the time complexity is 0(n).
4. Judging from the advantages and disadvantages of the linked list, the point of the linked list is that insertion and deletion are fast, so the linked list is used for more addition and deletion operations, and the array is used for more queries.
