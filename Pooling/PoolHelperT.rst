PoolHelper<T>
=============

.. highlight:: csharp

A global pool manager for regular objects with empty constructors.

If you need it for GameObjects take a look at the non-generic :doc:`PoolHelper`.

Usage
-----
Spawning
~~~~~~~~
::

    SomeClass someObject = PoolHelper<SomeClass>.Spawn();

Despawning
~~~~~~~~~~
::

    PoolHelper<SomeClass>.Despawn(someObject);

You can also despawn objects after a delay::

    // despawns after 1 sec in scaled time
    PoolHelper<SomeClass>.Despawn(someObject, 1f);

    // unscaled time
    PoolHelper<SomeClass>.Despawn(someObject, 1f, true);

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

        void Fire()
        {
            var bullet = PoolHelper<Bullet>.Spawn();
            bullet.Position = muzzlePosition;
            bullet.Velocity = direction * bulletSpeed;
            bullet.Impact += OnImpact;

            // despawn the bullet after a delay if it hasn't already been despawned
            PoolHelper<Bullet>.Despawn(bullet, bulletLifeTime);
        }

        void OnImpact(Bullet bullet, object otherObject)
        {
            bullet.Impact -= OnImpact;
            PoolHelper<Bullet>.Despawn(bullet);
        }
    }