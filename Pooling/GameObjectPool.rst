GameObjectPool
==============

.. highlight:: csharp

A subclass of :doc:`Pool\<T\> <Pool>` that supports GameObjects.

If the objects implement the :doc:`IPoolable` interface they will be notified when they're spawned and despawned.

If you need multiple :doc:`IPoolable` components in an instance to be notified take a look at the :doc:`PoolableGroup` component.

Multiscene support
------------------
When a pool is created you can set multiscene support to either off or on. 

If it's off the pool will reside in the current active scene. If the scene is unloaded the pool itself along with any despawned instances will be destroyed along with the scene.

If it's on, it will instead be marked as *DontDestroyOnLoad* and despawned instances will survive any scene unloading.

Usage
-----
Creating the pool
~~~~~~~~~~~~~~~~~
::

    var myPool = new GameObjectPool<SomeMonobehaviour>(somePrefab, multiScene:true);

We can also control the instantiating ourselves by using the overload with a factory method:

::

    var myPool = new GameObjectPool<SomeMonobehaviour>("MyPoolName", multiScene:true, () =>
    {
        var instance = Object.Instantiate(somePrefab);
        instance.SomeFloat = 3f;
        instance.SomeMethod();
        return instance;
    });

Spawning
~~~~~~~~
::

    SomeMonobehaviour somePrefabInstance = myPool.Spawn();

    // or

    var position = new Vector3(0, 0, 5);
    var rotation = Quaternion.Euler(0, 0, 90);
    var parent = someTransform;
    SomeMonobehaviour somePrefabInstance = myPool.Spawn(position, rotation, parent);

Despawning
~~~~~~~~~~
::

    myPool.Despawn(somePrefabInstance);

You can also despawn objects after a delay::

    // despawns after 1 sec in scaled time
    myPool.Despawn(somePrefabInstance, 1f);

    // unscaled time
    myPool.Despawn(somePrefabInstance, 1f, true);

Destroying the pool
~~~~~~~~~~~~~~~~~~~
You can destroy the pool and any despawned items it contains by calling *Dispose()*.

::

    myPool.Dispose();
    myPool = null;

Examples
--------
Simple bullet pool
~~~~~~~~~~~~~~~~~~
::

    public class Bullet : MonoBehaviour
    {
        public Vector3 Position;
        public Vector3 Velocity;
        public event Action<Bullet, Collider> Impact;

        void Update()
        {
            Position += Velocity * Time.deltaTime;
        }

        void OnTriggerEnter(Collider otherCollider)
        {
            Impact(this, otherCollider);
        }
    }

    public class Gun : MonoBehaviour
    {
        public float BulletSpeed = 5;
        public float BulletLifeTime = 10;
        public Bullet BulletPrefab;

        private GameObjectPool<Bullet> _bulletPool;

        void Awake()
        {
            _bulletPool = new GameObjectPool<Bullet>(BulletPrefab, multiScene:false);
        }

        void OnDestroy()
        {
            _bulletPool.Dispose();
            _bulletPool = null;
        }

        void Fire()
        {
            var bullet = _bulletPool.Spawn();
            bullet.Position = transform.position;
            bullet.Velocity = transform.forward * BulletSpeed;
            bullet.Impact += OnImpact;

            // despawn the bullet after a delay if it hasn't already been despawned
            _bulletPool.Despawn(bullet, BulletLifeTime);
        }

        void OnImpact(Bullet bullet, Collider otherCollider)
        {
            bullet.Impact -= OnImpact;
            _bulletPool.Despawn(bullet);
        }
    }