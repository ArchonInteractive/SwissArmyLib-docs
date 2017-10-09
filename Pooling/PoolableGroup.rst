PoolableGroup
=============

.. highlight:: csharp

A helpful component in case you want multiple :doc:`IPoolable` components to receive callbacks.

It manages a list of :doc:`IPoolable` components in it's children and serializes the references to avoid having to do so at runtime for each instantiated object.

Usage
-----
Just add it to the root the prefab and use it as the prefab reference for the pool. 

::

    var instance = PoolHelper.Spawn<PoolableGroup>(myPrefab);

    PoolHelper.Despawn<PoolableGroup>(instance);