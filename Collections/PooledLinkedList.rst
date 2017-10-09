PooledLinkedList<T>
===================

.. highlight:: csharp

PooledLinkedList<T> is a wrapper for LinkedList<T> that recycles its LinkedListNode<T> instances to reduce GC allocations.

You can either have each PooledLinkedList<T> have their own pool, or make them use a shared one.

Usage
-----
The API is identical to LinkedList<T> except for two extra constructors, so check out its `MSDN documentation <https://msdn.microsoft.com/en-us/library/he2s3bh7(v=vs.90).aspx>`_.

Shared pool
~~~~~~~~~~~
If you do not pass in a node pool in the constructor, it will create its own pool. If you have a bunch of lists with the same type of item, you might want to consider making them share a single node pool.

::

    Pool<LinkedListNode<int>> nodePool = new Pool<LinkedListNode<int>>(() => new LinkedListNode<int>());

    PooledLinkedList<int> list1 = new PooledLinkedList<int>(nodePool);
    PooledLinkedList<int> list2 = new PooledLinkedList<int>(nodePool);