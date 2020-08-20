# ArrayList vs LinkedList

Both ArrayList and LinkedList implement Java `List` interface, and their functions seem to be very similar. This article would compare the differences between them.

## Main Differences in Implementation 

ArrayList is implemented as a `resizable-array`, while LinkedList is implemented as a `doubly-linked list`. LinkedList also implements `Queue` interface, which means it provides Queue's methods like `peek()` and `poll()` as well.

## ArrayList

As I mentioned earlier, ArrayList uses an array to store data.

```
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

transient Object[] elementData;

public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

As we all know, the size of an array is fixed. When the original array's size is not enough, ArrayList would create a larger one and transfer data to it. Here is the source code:

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

If the `newCapacity` is still not large enough, it will be set to `minCapacity`:`newCapacity = minCapacity`.

The maximum size of the array is `Integer.MAX_VALUE`.   

It is recommended that we should define the size of the ArrayList if we know it in advance since the time spent in enlarging the array and copying data could be saved :`public ArrayList(int initialCapacity)`.