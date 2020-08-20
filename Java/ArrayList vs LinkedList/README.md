# ArrayList vs LinkedList

Both ArrayList and LinkedList implement Java `List` interface, and their functions seem to be very similar. This article would compare the differences between them.

## Main Differences in Implementation 

ArrayList is implemented as a `resizable-array`, while LinkedList is implemented as a `doubly-linked list`. LinkedList also implements `Queue` interface, which means it provides Queue operations like `peek()` and `poll()` as well.

## ArrayList

### Capacity   

   
As I mentioned earlier, ArrayList uses an array to store data.

```
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

transient Object[] elementData;

public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

As we all know, the size of an array is fixed. When the original array's size is not enough, ArrayList would create a larger one and transfer data to it :

```
/**
* Increases the capacity to ensure that it can hold at least the
* number of elements specified by the minimum capacity argument.
*
* @param minCapacity the desired minimum capacity
*/
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

We can see that initially it would increase its capacity to 150% : `int newCapacity = oldCapacity + (oldCapacity >> 1)`.   

If the `newCapacity` is still not large enough, it will be set to minCapacity : `newCapacity = minCapacity`.

The maximum size of the array is `Integer.MAX_VALUE`.   

It is recommended that we should define the size of the ArrayList if we know it in advance : `public ArrayList(int initialCapacity)`. This is because the time spent in enlarging the array and copying data could be saved.

### Add Data

There are two methods that implement this function:`public boolean add(E e)`&`public void add(int index, E element)`.   

For `public boolean add(E e)`, the new data would become the last element of the array :

```
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}
``` 

For `public void add(int index, E element)`, it would first move the data in `elementData` from `[index, element.length - 1]` to `[index + 1, element.length]`, and put the new element in `elementData[index]`:

```
public void add(int index, E element) {
    rangeCheckForAdd(index);

    ensureCapacityInternal(size + 1);  // Increments modCount!!
    System.arraycopy(elementData, index, elementData, index + 1, size - index);
    elementData[index] = element;
    size++;
}
```

### Remove Data

We can see that in order to remove elementData[index], elementData[index + 1, size - 1] also get involved (need to be left shifted) : 

```
public E remove(int index) {
    rangeCheck(index);

    modCount++;
    E oldValue = elementData(index);

    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index, numMoved);
    
    elementData[--size] = null; // clear to let GC do its work
    
    return oldValue;
}
```

## LinkedList

### Add Data

The new element would become the last node of the list :

```
public boolean add(E e) {
     linkLast(e);
     return true;
}
```

### Get Data

We can see that LinkedList needs to traverse the doubly-linked list so as to get the `index` Node :

```
public E get(int index) {
    checkElementIndex(index);
    return node(index).item;
}

Node<E> node(int index) {
    // assert isElementIndex(index);

    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```

### Queue Operations

As I mentioned earlier, LinkedList also implements `Queue` interface. Some of its implementations are as follows :   

`peek()`: Retrieves, but does not remove, the head (first element) of this list.
``` 
public E peek() {
    final Node<E> f = first;
    return (f == null) ? null : f.item;
}
```
`poll()`: Retrieves and removes the head (first element) of this list.
```
public E poll() {
    final Node<E> f = first;
    return (f == null) ? null : unlinkFirst(f);
}
```
`remove()`: Retrieves and removes the head (first element) of this list.
```
public E remove() { return removeFirst(); }
```
`offer(E e)`: Adds the specified element as the tail (last element) of this list. We can see that there is no difference between `offer(E e)` and `add(E e)`.
```
public boolean offer(E e) { return add(e); }
```
`push(E e)`: inserts the element at the front of this list.
```
public void push(E e) { addFirst(e); }
```
`pop()`: removes and returns the first element of this list.
```
public E pop() { return removeFirst(); }
```

## Conclusion

1. ArrayList would become a better choice if there are a large number of random access of element, since ArrayList just gets the value from `elementData[]`, while LinkedList needs to traverse the doubly-linked list.

2. LinkedList would do better in adding or removing elements since it does not involve the operations of moving or copying data.
