Object Pooling
==============

.. toctree::
    :hidden:
    
    PoolHelper
    PoolHelperT
    Pool
    GameObjectPool
    IPoolable
    PoolableGroup

A simple object pooling solution for both regular objects as well as GameObjects. 

+---------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
| Class                                       | Purpose                                                                                                                |
+=============================================+========================================================================================================================+
| :doc:`PoolHelper`                           | A convenient global pool manager for GameObjects.                                                                      |
+---------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
| :doc:`PoolHelper\<T\> <PoolHelperT>`        | A convenient global pool manager for regular objects with empty constructors.                                          |
+---------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
| :doc:`Pool\<T\> <Pool>`                     | A simple object pool for any type of objects that supports custom factory methods, event callbacks and timed despawns. |
+---------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
| :doc:`GameObjectPool\<T\> <GameObjectPool>` | A subclass of Pool<T> that supports GameObjects.                                                                       |
+---------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
| :doc:`IPoolable`                            | An interface that poolable classes can implement to get notified when they're spawned or despawned.                    |
+---------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
| :doc:`PoolableGroup`                        | A helpful component in case you want multiple IPoolable components to receive callbacks.                               |
+---------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
