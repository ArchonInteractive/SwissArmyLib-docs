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
* ManagedUpdate.OnFrameIntervalUpdate
* ManagedUpdate.OnTimeIntervalUpdate

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
* ManagedUpdate.OnFrameIntervalUpdate
* ManagedUpdate.OnTimeIntervalUpdate

Interval Updates
~~~~~~~~~~~~~~~~
In addition to the regular Unity update loops, ManagedUpdate also support an extra two: frame-based interval and time-based interval.
These loops run at a configurable interval (see **ManagedUpdate.FrameInterval** and **ManagedUpdate.TimeInterval**).
If you want to know the time difference since these updates were last invoked, you can call **ManagedUpdate.DeltaTime** or **ManagedUpdate.UnscaledDeltaTime**.

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