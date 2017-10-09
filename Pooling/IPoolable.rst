IPoolable
=========

.. highlight:: csharp

An interface that poolable classes can implement to get notified when they're spawned or despawned.

Need multiple MonoBehaviours to get notified when their prefab instance is spawned or despawned then look at :doc:`PoolableGroup`.

Usage
-----
Just implement the interface in the pooled class.

::

    public class MyPooledClass : IPoolable
    {
        public void OnSpawned()
        {
            Debug.Log("I'm alive!");
        }

        public void OnDespawned()
        {
            Debug.Log("Back into the pool I go :(");
        }
    }

    // somewhere else
    var myInstance = PoolHelper<MyPooledClass>.Spawn();
    Debug.Log("I just spawned myInstance");

    PoolHelper<MyPooledClass>.Despawn(myInstance);
    Debug.Log("I just despawned myInstance");

Will output:

.. sourcecode:: none

    I'm alive!
    I just spawned myInstance
    Back into the pool I go :(
    I just despawned myInstance