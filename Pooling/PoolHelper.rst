PoolHelper
==========

.. highlight:: csharp

A global pool manager for GameObjects.

If you need it for regular objects take a look at the generic version :doc:`PoolHelper\<T\> <PoolHelperT>`.

Usage
-----
Spawning
~~~~~~~~
::

    SomeMonobehaviour somePrefabInstance = PoolHelper.Spawn<SomeMonobehaviour>(myPrefab);

Despawning
~~~~~~~~~~
::

    PoolHelper.Despawn(somePrefabInstance);

You can also despawn objects after a delay::

    // despawns after 1 sec in scaled time
    PoolHelper.Despawn(somePrefabInstance, 1f);

    // unscaled time
    PoolHelper.Despawn(somePrefabInstance, 1f, true);

Examples
--------
Simple bullet pool
~~~~~~~~~~~~~~~~~~
::

    public class Bullet : MonoBehaviour, IPoolable
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

        void IPoolable.OnSpawned()
        {

        }

        void IPoolable.OnDespawned()
        {
            // reset state
            Position = Vector3.zero;
            Velocity = Vector3.zero;
            Impact = null;
        }
    }

    public class Gun : MonoBehaviour
    {
        public float BulletSpeed = 5;
        public float BulletLifeTime = 10;
        public Bullet BulletPrefab;

        void Fire()
        {
            var bullet = PoolHelper.Spawn(BulletPrefab);
            bullet.Position = transform.position;
            bullet.Velocity = transform.forward * BulletSpeed;
            bullet.Impact += OnImpact;

            // despawn the bullet after a delay if it hasn't already been despawned
            PoolHelper.Despawn(bullet, BulletLifeTime);
        }

        void OnImpact(Bullet bullet, Collider otherCollider)
        {
            // inflict damage or something

            PoolHelper.Despawn(bullet);
        }
    }