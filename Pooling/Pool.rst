Pool<T>
=======

.. highlight:: csharp

A simple object pool for any type of objects that supports custom factory methods, event callbacks and timed despawns.

If the objects implement the :doc:`IPoolable` interface they will be notified when they're spawned and despawned.

Usage
-----
Creating the pool
~~~~~~~~~~~~~~~~~
::

    Pool<SomeClass> myPool = new Pool<SomeClass>(() => new SomeClass());

Since we use a delegate for creating the instances, you are not limited to just using the empty constructor::

    Pool<SomeClass> myPool = new Pool<SomeClass>(() =>
    {
        var instance = new SomeClass();
        instance.SomeFloat = 3f;
        instance.SomeMethod();
        return instance;
    });

Spawning
~~~~~~~~
::

    SomeClass someObject = myPool.Spawn();

Despawning
~~~~~~~~~~
::

    myPool.Despawn(someObject);

You can also despawn objects after a delay::

    // despawns after 1 sec in scaled time
    myPool.Despawn(someObject, 1f);

    // unscaled time
    myPool.Despawn(someObject, 1f, true);

Examples
--------
Simple bullet pool
~~~~~~~~~~~~~~~~~~
::

    public class Bullet
    {
        public Vector3 Position;
        public Vector3 Velocity;
        public event Action<Bullet, object> Impact;

        void Update()
        {
            Position += Velocity * Time.deltaTime;

            if (weHitSomething)
                Impact(this, whatWeHit);
        }
    }

    public class Gun
    {
        Vector3 muzzlePosition;
        Vector3 direction;
        float bulletSpeed = 5;
        float bulletLifeTime = 10;

        Pool<Bullet> _bulletPool = new Pool<Bullet>(() => new Bullet());

        void Fire()
        {
            var bullet = _bulletPool.Spawn();
            bullet.Position = muzzlePosition;
            bullet.Velocity = direction * bulletSpeed;
            bullet.Impact += OnImpact;

            // despawn the bullet after a delay if it hasn't already been despawned
            _bulletPool.Despawn(bullet, bulletLifeTime);
        }

        void OnImpact(Bullet bullet, object otherObject)
        {
            bullet.Impact -= OnImpact;
            _bulletPool.Despawn(bullet);
        }
    }