DelayedList<T>
==============

.. highlight:: csharp

A list wrapper that delays adding or removing items from the list until its method *ProcessPending()* is called. Useful to avoid problems when you want to be able to change a list in the midst of iterating through it. 

Usage
-----
Creation
~~~~~~~~
::

    DelayedList<object> myList = new DelayedList<object>();

You can also pass in a **IList<T>** instance if you need it to be something other than a regular **List<T>**.

::

    IList<object> myCustomList = new SomeOtherListType<object>();
    DelayedList<object> myList = new DelayedList<object>(myCustomList);

Processing changes
~~~~~~~~~~~~~~~~~~
Once you want pending changes to be applied to the list (eg. before iterating through the items), you should call *ProcessPending()*.

Pending changes will be executed in the order they were originally called.

::

    DelayedList<object> myList = new DelayedList<object>();

    myList.Add(someObject);
    Debug.Log(myList.Count);

    myList.ProcessPending();
    Debug.Log(myList.Count);

Will output:

::

    0
    1

Adding
~~~~~~
::

    myList.Add(someObject);

After adding an item they still won't be in the list until you call *ProcessPending()*.

Removing
~~~~~~~~
::

    myList.Remove(someObject);

Or by index::

    myList.RemoveAt(3);

After removing an item they will still be in the list until you call *ProcessPending()*.

Clearing
~~~~~~~~
You can also queue up clearing the list, removing every previous pending change and all items currently contained in the list the next time *ProcessPending()* is called.

If you add any other changes after calling *Clear()* they will get added as expected once *ProcessPending()* is called.

::

    myList.Clear();

If you don't want to have to call ProcessPending and just have all items and pending changes cleared instantly, you can instead call the aptly named *ClearInstantly()*.

Do not however that you should naturally not call this while iterating through the items.

::

    myList.ClearInstantly();

If you only want to clear the pending changes and keep the contents the same, you can call *ClearPending()* at any time.

::

    myList.ClearPending();

Examples
--------
.. todo::
    Write some DelayedList examples.