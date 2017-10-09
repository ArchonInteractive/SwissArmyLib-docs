ManagedUpdateBehaviour
======================

.. highlight:: csharp

An abstract subclass of **MonoBehaviour** that uses :doc:`ManagedUpdate` for update events.

Usage
-----
Just inherit your **MonoBehaviours** from **ManagedUpdateBehaviour** instead and implement one (or more) of the interfaces: **IUpdateable**, **ILateUpdateable** and **IFixedUpdateable**.

Instead of Unity's magic *Update()*, *LateUpdate()* and *FixedUpdate()* methods you should use the corresponding methods from the interfaces.

If you override *Start()*, *OnEnable()* or *OnDisable()* make sure you remember to call the base method, to save yourself a debugging headache.

Execution Order
~~~~~~~~~~~~~~~
While **ManagedUpdateBehaviour** doesn't respect Unity's ScriptExecutionOrder, it does however have its own counterpart. Do however note that this is completely separate from ScriptExecutionOrder and it only affects the order that **ManagedUpdateBehaviours** will have their methods called.

To use it just override the *ExecutionOrder* property like so::

    // Will now be called before other behaviours with a higher ExecutionOrder
    protected override int ExecutionOrder { get { return -1000; } }

Examples
--------
Simple2DMovement
~~~~~~~~~~~~~~~~
::

    public class Simple2DMovement : ManagedUpdateBehaviour, IUpdateable
    {
        public float Speed = 5;

        public void OnUpdate()
        {
            var dir = new Vector3(Input.GetAxis("Horizontal"), Input.GetAxis("Vertical"), 0);
            transform.position += dir * Speed * Time.deltaTime;
        }
    }