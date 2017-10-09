TellMeWhen
==========

.. highlight:: csharp

**TellMeWhen** is a simple utility for whenever you need something to be called after a certain time. Instead of relying on Unity's (relatively expensive) *Invoke()*, a coroutine with a *WaitForSeconds()* or simply checking each update, you can just ask **TellMeWhen** to notify when the time has passed. Optionally you can pass some arguments along with the notification or make it happen repeatedly every Nth second.

Usage
-----
Scheduling
~~~~~~~~~~
First step is implementing the **ITimerCallback** interface in whatever class you want to be able to receive the timer callbacks.

Then all you have to do is call one of the static **TellMeWhen** scheduling methods::

	TellMeWhen.Exact(time, listener, id, args);
	TellMeWhen.Seconds(seconds, listener, id, args);
	TellMeWhen.Minutes(minutes, listener, id, args);

	TellMeWhen.ExactUnscaled(time, listener, id, args);
	TellMeWhen.SecondsUnscaled(seconds, listener, id, args);
	TellMeWhen.MinutesUnscaled(minutes, listener, id, args);
	
The 'normally' named methods use Unity's ol' **Time.time**, which is scaled according to **Time.timeScale**. If you use that for pausing the game, but don't want your timers to be delayed accordingly, you might want to use the *Unscaled* methods. These will as the name says use **Time.unscaledTime** instead, which completely ignore the time scale.
	
Multiple timers
~~~~~~~~~~~~~~~
If you have multiple different timers for the same listener you might want to differentiate by an id. The scheduling methods take an optional id that will be passed to the listener when the timer is triggered.

Arguments
~~~~~~~~~
You can optionally pass an object along to the listener when scheduling.

Canceling
~~~~~~~~~
You can cancel a specific timer by calling *Cancel(listener, eventId)* or just clear everything for a listener by calling *Cancel(listener)*.

Examples
--------
Single timer, no args
~~~~~~~~~~~~~~~~~~~~~
::

	public class Bomb : MonoBehaviour, ITimerCallback
	{
		public float CountdownTime = 5;

		private void Awake()
		{
			TellMeWhen.Seconds(CountdownTime, this);
		}

		public void OnTimesUp(int id, object args)
		{
			// boom!
		}
	}

Single repeating timer
~~~~~~~~~~~~~~~~~~~~~~
::

	public class DamageOverTime : MonoBehaviour, ITimerCallback
	{
		public float DamagePerSecond = 1;
		
		private void OnEnable()
		{
			TellMeWhen.Seconds(1, this, repeating:true);
		}
		
		private void OnDisable()
		{
			// clears all timers for this listener (but of course we only have one anyway)
			TellMeWhen.Cancel(this);
		}
		
		// called every second
		public void OnTimesUp(int id, object args)
		{
			ImaginaryHealth.Damage(DamagePerSecond);
		}
	}

Single timer with args
~~~~~~~~~~~~~~~~~~~~~~
::

	public class GameObjectPool : ITimerCallback
	{
		public void Despawn(GameObject gameObject)
		{
			// disable gameObject and back to pool
		}

		public void DelayedDespawn(GameObject gameObject, float delay)
		{
			TellMeWhen.Seconds(delay, this, args:gameObject);
		}
		
		public void OnTimesUp(int id, object args)
		{
			var target = args as GameObject;
			Despawn(target);
		}
		
		// ... yadda yadda rest of pool
	}

Multiple timers
~~~~~~~~~~~~~~~
::

	public class Bomb : MonoBehaviour, ITimerCallback
	{
		public float CountdownTime = 5;
		
		private static class Timers
		{
			public const int Tick = 0,
					 Explode = 1;
		}

		private void Awake()
		{
			TellMeWhen.Seconds(1, this, Timers.Tick, repeating: true);
			TellMeWhen.Seconds(CountdownTime, this, Timers.Explode);
		}

		public void OnTimesUp(int id, object args)
		{
			switch (id)
			{
				case Timers.Tick:
					// play ticking animation
					break;
				case Timers.Explode:
					// since Timers.Tick is repeating, we have to cancel it
					// so it doesn't continue after the big boom boom
					TellMeWhen.Cancel(Timers.Tick, this);
					// boom!
					break;
			}
		}
	}