GlobalEvents
============

.. highlight:: csharp

A manager of events that do not belong to any specific object but instead can be listened to by anyone and invoked by anyone.

Useful for GameLoaded, MatchEnded and similar events.

This uses :doc:`Event` instances behind the scene.

If you need to pass along some data to the listeners, you can use the alternative **GlobalEvents<T>** version.

Events are differentiated by an integer. You are expected to create constants to define your events.

If you just need regular local events, take a took at :doc:`Event`.

Usage
-----
Invoking
~~~~~~~~
Invoking an event is as simple as calling::

    GlobalEvents.Invoke(eventId);

Subscribing
~~~~~~~~~~~
Interface listener
__________________
First of all you have to implement the **IEventListener** interface, which has an *OnEvent(int eventId)* method that will be called, as the name suggests, when an event is invoked.

Then add yourself as a listener for an event::

    GlobalEvents.AddListener(eventId, listener);

You can optionally pass in a priority if you want your listener to be notified before or after other listeners.

And that's it! Your *OnEvent(id)* method will be called when the event is invoked.

Delegate listener
_________________
Using a delegate listener is simpler and works for static classes as well, but also less performant since they often allocate short-living memory.

::

    void MyMethod()
    {
    }

    GlobalEvents.AddListener(eventId, MyMethod);

Unsubscribing
~~~~~~~~~~~~~
Removing yourself as a listener is even easier, simply call::

    GlobalEvents.RemoveListener(eventId, listener);

Or if you want your listener to be unsubscribed from all events you can call::

    GlobalEvents.RemoveListener(listener);

Clearing
~~~~~~~~
In case you ever need to completely clear all listeners for an event::

    GlobalEvents.Clear(eventId);

Or maybe you want a completely clean slate::

    GlobalEvents.Clear();

Examples
--------
Global events without args
~~~~~~~~~~~~~~~~~~~~~~~~~~
In this example we want the UI to be notified when the player dies or is revived, so we can show or hide the death screen accordingly.

::

    public static class EventIds
    {
        public const int PlayerDied = 0,
                         PlayerRevived = 1;
    }

    public class Player : MonoBehaviour
    {
        public void Kill()
        {
            // play death animation

            GlobalEvents.Invoke(EventIds.PlayerDied);
        }

        public void Revive()
        {
            // praise the lord, we were revived!

            GlobalEvents.Invoke(EventIds.PlayerRevived);
        }
    }

    public class UIManager : MonoBehaviour, IEventListener
    {
        private void OnEnable()
        {
            GlobalEvents.AddListener(EventIds.PlayerDied, this);
            GlobalEvents.AddListener(EventIds.PlayerRevived, this);
        }

        private void OnDisable()
        {
            GlobalEvents.RemoveListener(EventIds.PlayerDied, this);
            GlobalEvents.RemoveListener(EventIds.PlayerRevived, this);
        }

        public void OnEvent(int eventId)
        {
            switch (eventId)
            {
                case PlayerRevived:
                    HideDeathScreen();
                    break;
                case PlayerDied:
                    ShowDeathScreen();
                    break;
            }
        }
    }

Global event with args
~~~~~~~~~~~~~~~~~~~~~~
This time we have a multiplayer game and need to keep track of player stats: Kills and deaths. Instead of having the player class find and call the scoreboard itself, we make the scoreboard rely on a PlayerKilled event instead. When a player is killed it invokes the event and sends along information about the event (who the victim is, and who the killer is). This way other systems that might need to know when players are killed, can be notified too without even having to change the Player class again.

::

    public class PlayerKilledArgs : EventArgs
    {
        public Player Victim;
        public Player Killer;
    }

    public static class EventIds
    {
        public const int PlayerKilled = 0;
    }

    public class Player : MonoBehaviour
    {
        // we store it in a field, so we can reuse the same instance and avoid generating garbage
        private PlayerKilledArgs _playerKilledArgs = new PlayerKilledArgs();

        public void Kill(Player killer)
        {
            // play death animation

            // update event args
            _playerKilledArgs.Victim = this;
            _playerKilledArgs.Killer = killer;

            GlobalEvents<EventArgs>.Invoke(EventIds.PlayerKilled, _playerKilledArgs);
        }
    }

    public class Scoreboard : MonoBehaviour, IEventListener<EventArgs>
    {
        private void OnEnable()
        {
            GlobalEvents<EventArgs>.AddListener(EventIds.PlayerKilled, this);
        }

        private void OnDisable()
        {
            GlobalEvents<EventArgs>.RemoveListener(EventIds.PlayerKilled, this);
        }

        public void OnEvent(int eventId, EventArgs args)
        {
            if (eventId == EventIds.PlayerKilled)
            {
                var playerKilledArgs = (PlayerKilledArgs) args;

                AddKill(PlayerKilledArgs.Killer);
                AddDeath(PlayerKilledArgs.Victim);
            }
        }
    }

.. todo::
    Examples for:

    * Global events with prioritized listeners