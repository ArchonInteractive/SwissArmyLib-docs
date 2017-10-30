ManagedUpdate
=============

.. highlight:: csharp

A relay for Unity update events.
Also useful for non-**MonoBehaviours** that needs to be part of the update loop as well.

Check out :doc:`ManagedUpdateBehaviour` for a simple **MonoBehaviour** subclass that makes use of this instead of regular events.

Events that you can listen to:

* ManagedUpdate.OnUpdate
* ManagedUpdate.OnLateUpdate
* ManagedUpdate.OnFixedUpdate

You can also create custom update loops that for example runs once every second.

Why?
----
`Here's why you might want to use this. <https://blogs.unity3d.com/2015/12/23/1k-update-calls/>`_
In short; avoid overhead of Native C++ --> Managed C# calls.

Usage
-----
:ref:`Subscribing <event-subscribing>` to one of the relayed events:

* ManagedUpdate.OnUpdate
* ManagedUpdate.OnLateUpdate
* ManagedUpdate.OnFixedUpdate
  
You can also use the method *AddListener(eventId, listener)* in ManagedUpdate but it's only required if you use custom update loops.

Custom Update Loops
~~~~~~~~~~~~~~~~
In addition to the regular Unity update loops, ManagedUpdate also supports custom ones that you can control when runs.
You can create such a loop by creating an instance of a class that implements the **ICustomUpdateLoop** interface (though it's simpler if you just subclass **CustomUpdateLoopBase**).
There's two simple implementations already available if you need them: **TimeIntervalUpdateLoop** and **FrameIntervalUpdateLoop**.

::

    var updateLoop = new TimeIntervalUpdateLoop(eventId: 1000, interval: 1);

Then add the instance to ManagedUpdate by calling *AddCustomUpdateLoop()*.

::

    ManagedUpdate.AddCustomUpdateLoop(updateLoop);

Subscribing to a custom update loop requires using the *AddListener(eventId, listener)* in ManagedUpdate (or you could use the updateloop instance directly).

::

    Action listener = () => Debug.Log("Booyah!");
    ManagedUpdate.AddListener(eventId: 1000, listener);

If you want to know the time difference since these updates were last invoked, you can call **ManagedUpdate.DeltaTime** or **ManagedUpdate.UnscaledDeltaTime**.

::

    Action listener = () => 
    {
        Debug.LogFormat("It has been {0} seconds since the last update!", ManagedUpdate.DeltaTime);
    };
    ManagedUpdate.AddListener(eventId: 1000, listener);

Examples
--------
MonoBehaviour
~~~~~~~~~~~~~
Check out :doc:`ManagedUpdateBehaviour` if you need something like this.

::

    public class SimpleManagedUpdateBehaviour : MonoBehaviour, IEventListener
    {
        protected virtual void OnEnable()
        {
            ManagedUpdate.OnUpdate.AddListener(this);
        }

        protected virtual void OnDisable()
        {
            ManagedUpdate.OnUpdate.RemoveListener(this);
        }

        protected virtual void OnEvent(int eventId)
        {
            if (eventId == ManagedUpdate.EventIds.Update)
            {
                // regular ol' update logic
            }
        }
    }

Non-MonoBehaviour
~~~~~~~~~~~~~~~~~
There's not much to it, you just need to subscribe *somewhere*.
In this example we just control whether the class receives updates through its *Enabled* property.

::

    public class ManagedUpdateListener : IEventListener
    {
        private bool _enabled = false;
        public bool Enabled 
        { 
            get { return _enabled; }
            set 
            {
                if (_enabled == value)
                    return;

                _enabled = value;

                if (_enabled)
                    ManagedUpdate.OnUpdate.AddListener(this);
                else
                    ManagedUpdate.OnUpdate.RemoveListener(this);
            }
        }

        void OnEvent(int eventId)
        {
            if (eventId == ManagedUpdate.EventIds.Update)
            {
                // regular ol' update logic
            }
        }
    }