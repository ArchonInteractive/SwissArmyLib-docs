Event
=====

.. highlight:: csharp

A simple event handler that supports both interface and delegate listeners. Listeners can have a specific priority that affects whether they're called before or after other listeners. If multiple listeners have the same priority, they will be called in the order they were added.

All listeners will be notified even if one of the invocations throws an exception.

If you need to pass along some data to the listeners, you can use the alternative **Event<T>** version.

Events are differentiated by an integer. You are expected to define them as constants and not just keeping track of the ids in your head.

If you need global events, take a took at :doc:`GlobalEvents`.

Usage
-----
Creating
~~~~~~~~
Define your event's id somewhere, eg. in a static EventIds class.

::

    public static class EventIds
    {
        public const int MyEvent = 0;
    }

Now create the event handler.

::

    public class SomeClassWithAnEvent
    {
        public readonly Event MyEventWithoutArgs = new Event(EventIds.MyEvent);

        // or if you need args:

        public readonly Event<int> MyEventWithArgs = new Event<int>(EventIds.MyEvent);
    }

That's it, the event handlers are ready to be used.

Invoking
~~~~~~~~
Invoking an event is as simple as calling::

    MyEventWithoutArgs.Invoke();

    // or with args:
    MyEventWithArgs.Invoke(10);

.. _event-subscribing:

Subscribing
~~~~~~~~~~~

Interface listener
__________________
First of all you have to implement the **IEventListener** (no args) or **IEventListener<T>** (with args) interface, which has an *OnEvent()* method that will be called, as the name suggests, when an event is invoked.

::

    public class SomeListenerWithoutArgs : IEventListener
    {
        public void OnEvent(int eventId)
        {

        }
    }

    public class SomeListenerWithArgs : IEventListener<int>
    {
        public void OnEvent(int eventId, int args)
        {

        }
    }

Then add yourself as a listener for the event::

    MyEvent.AddListener(myListener);

And that's it! Your *OnEvent()* method will be called when the event is invoked.

Delegate listener
_________________
Using a delegate listener is simpler and works for static classes as well, but also less performant since they often allocate short-living memory.

First define the methods you want called::

    void OnMyEventWithoutArgs()
    {

    }

    void OnMyEventWithArgs(int args)
    {

    }

Then add them as listeners to the events::

    MyEventWithoutArgs.AddListener(OnMyEventWithoutArgs);
    MyEventWithArgs.AddListener(OnMyEventWithArgs);


Prioritization
~~~~~~~~~~~~~~
You can optionally pass in a priority if you want your listener to be notified before or after other listeners.

::

    MyEvent.AddListener(myListener);
    MyEvent.AddListener(myOtherListener, -1000);

*myOtherListener* will now get called before *myListener*, despite being added later.

Unsubscribing
~~~~~~~~~~~~~
Removing yourself as a listener is even easier, simply call::

    MyEvent.RemoveListener(myListener);

Clearing
~~~~~~~~
In case you ever need to completely clear all listeners for an event::

    MyEvent.Clear();

.. todo::
    Examples for:

    * Events without args
    * Events with args
    * Events with prioritized listeners