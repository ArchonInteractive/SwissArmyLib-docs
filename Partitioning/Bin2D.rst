Bin2D<T>
========

.. highlight:: csharp

A simple GC-friendly two-dimensional `Bin (aka Spatial Grid) <https://en.wikipedia.org/wiki/Bin_(computational_geometry)>`_ implementation.

When you're done with the Bin, you should call *Dispose()* so its resources can be freed in their object pool. If you forget this, no harm will be done but memory will be GC'ed.

If you're doing 3D, take a look at :doc:`Bin3D\<T\> <Bin3D>`.

Usage
-----
Creation
~~~~~~~~
::

    int gridWidth = 25;
    int gridHeight = 20;
    float cellWidth = 1;
    float cellHeight = 1;

    Bin2D<object> bin = new Bin2D<object>(gridWidth, gridHeight, cellWidth, cellHeight);

When you no longer need the Bin2D, you should call *Dispose()* on it::

    bin.Dispose();

Retrieving potential matches
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can retrieve a set of objects that potentially intersects a rect. 

::

    HashSet<object> results = new HashSet<object>();

    Rect queryRect = Rect.MinMaxRect(4, 4, 9, 9);
    bin.Retrieve(queryRect, results);

You should recycle the results HashSet for future calls to avoid GC allocations. Remember to clear it before you reuse it.

Insertion
~~~~~~~~~
::

    object item = new object();
    Rect itemBounds = Rect.MinMaxRect(4, 4, 5, 5);

    bin.Insert(item, itemBounds);

Removing
~~~~~~~~
To remove an item you need to know the bounds that was used to add it.

::

    bin.Remove(item, itemBoundsWhenAdded);

In case you don't know the previous bounds you can use *Remove(item)* which goes through every cell and removes the item. It's much cheaper if you cache the bounds instead, but it's there if you need it.

::

    bin.Remove(item);

Updating
~~~~~~~~
Updating an item's bounds in the bin is easy. 
Just like when removing an item, you have to know the bounds that was used when adding it.

::

    bin.Update(item, oldItemBounds, newItemBounds);

Clearing
~~~~~~~~
You can clear all items from the bin with *Clear()*.

::

    bin.Clear();