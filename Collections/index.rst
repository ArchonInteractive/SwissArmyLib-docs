Collections
===========

.. toctree::
	:hidden:
	
	Grid2D
	Grid3D
	PooledLinkedList
	DelayedList
	PrioritizedList
	DictionaryWithDefault

There's a bunch of collection types available in **SwissArmyLib**, mostly just used internally but perhaps someone might find them useful.

===================================================================  =============================================================
Class                                                                     Purpose
===================================================================  =============================================================
:doc:`Grid2D\<T\> <Grid2D>`                                          A fixed (but resizable) 2D grid of items.
:doc:`Grid3D\<T\> <Grid3D>`                                          Same as Grid2D<T> but with an extra axis.
:doc:`PooledLinkedList\<T\> <PooledLinkedList>`                      A wrapper for LinkedList<T> that recycles its LinkedListNode<T> instances to reduce GC allocations.
:doc:`DelayedList\<T\> <DelayedList>`                                A list wrapper that delays adding or removing item from the list until its method *ProcessPending()* is called. Useful to avoid problems when you want to be able to add or remove from a list in the midst of iterating through it.
:doc:`PrioritizedList\<T\> <PrioritizedList>`                        A list where the items are kept sorted by their priorities.
:doc:`DictionaryWithDefault\<TKey,TValue\> <DictionaryWithDefault>`  Just a regular Dictionary<TKey,TValue> but instead of throwing an error when a key doesn't exist it just returns a specified default value.
===================================================================  =============================================================