BetterCoroutines
================

.. highlight:: csharp

Coroutines is an awesome hack, but unfortunately they're not that cheap. 
BetterCoroutines is a very similar but much more performant alternative to Unity's coroutines. It runs faster and reduces the amount of garbage generated.
Furthermore you have more control over which update loop they run in and their lifetime is not dependent on gameobjects. 

That said, they still aren't super cheap and should be used sparingly (if at all) if you're shooting for high performance. Their performance is similar to that of `More Effective Coroutines <http://trinary.tech/category/mec/>`_.

Usage
-----
Defining your coroutine
~~~~~~~~~~~~~~~~~~~~~~~
Defining your coroutine method is exactly the same as with Unity's coroutines::

    IEnumerator MyRoutine()
    {
        Debug.Log("Let's wait a frame!");
        yield return null;
        Debug.Log("That was awesome!");
    }

Starting a coroutine
~~~~~~~~~~~~~~~~~~~~
Instead of using *MonoBehaviour.StartCoroutine()* you should use *BetterCoroutines.Start()*.

::

    BetterCoroutines.Start(MyRoutine());

This'll run *MyRoutine* in the regular update loop.

If you instead want it to run in LateUpdate or FixedUpdate you can do the following::

    BetterCoroutines.Start(MyRoutine(), UpdateLoop.LateUpdate);

    // or

    BetterCoroutines.Start(MyRoutine(), UpdateLoop.FixedUpdate);

If you want your coroutines to stop automatically when a gameobject or component is destroyed/disabled, you can do so with an overload::

    BetterCoroutines.Start(MyRoutine(), myGameObject);

    // or

    BetterCoroutines.Start(MyRoutine(), myComponent);

Stopping a coroutine
~~~~~~~~~~~~~~~~~~~~
The start method actually returns the id of the started coroutine, which you can use to stop it prematurely if needed.

::

    int myCoroutineId = BetterCoroutines.Start(MyRoutine());

    // later

    bool wasStopped = BetterCoroutines.Stop(myCoroutineId);

*wasStopped* will now be true if the coroutine was found and stopped, otherwise it will be false.

Stopping all coroutines
~~~~~~~~~~~~~~~~~~~~~~~
If you suddenly have the urge to stop all coroutines you can do so easily::

    // stops all coroutines regardless of which update loop they're part of
    BetterCoroutines.StopAll();

    // stops all coroutines in a specific update loop
    BetterCoroutines.StopAll(UpdateLoop.FixedUpdate);

Checking whether a specific coroutine is running
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can check if a coroutine with a specific ID is currently running.

::

    bool isRunning = BetterCoroutines.IsRunning(myCoroutineId);

Pausing a coroutine
~~~~~~~~~~~~~~~~~~~
BetterCoroutines supports pausing a coroutine as well.

::

    BetterCoroutines.Pause(myCoroutineId);
    BetterCoroutines.Unpause(myCoroutineId);

    // or

    BetterCoroutines.SetPaused(myCoroutineId, true);
    BetterCoroutines.SetPaused(myCoroutineId, false);

You can can check whether a coroutine is currently paused with *IsPaused(id)*.

::

    bool isPaused = BetterCoroutines.IsPaused(myCoroutineId);

Yield Operations
~~~~~~~~~~~~~~~~

Wait a frame
____________
Just as with Unity's coroutines you can wait a single frame by yielding *null*.

If this is not verbose enough for you, you can instead use the constant BetterCoroutines.WaitForOneFrame.

::

    IEnumerator WaitAFrameRoutine()
    {
        Debug.Log("First frame");
        yield return null;
        Debug.Log("Second frame");
        yield return BetterCoroutines.WaitForOneFrame;
        Debug.Log("Three frame");
    }

Subcoroutines
_____________
You can yield a coroutine to suspend the current one until the yielded coroutine is done.

::

    IEnumerator MyRoutine()
    {
        yield return MySubroutine();

        // or

        yield return BetterCoroutines.Start(MySubroutine());
    }

    IEnumerator MySubroutine()
    {
        yield return null;
    }

WaitForSeconds
______________
You can suspend a coroutine for a certain time either in scaled or unscaled time.

::

    IEnumerator WaitForSecondsRoutine()
    {
        // waits 1 second in scaled time
        yield return BetterCoroutines.WaitForSeconds(1);

        // waits 1 second in unscaled time
        yield return BetterCoroutines.WaitForSeconds(1, false);
    }

WaitForSecondsRealtime
______________________
You can also suspend a coroutine for a specific time in realtime.

::

    IEnumerator WaitForSecondsRealtimeRoutine()
    {
        yield return BetterCoroutines.WaitForSecondsRealtime(1);
    }

WaitWhile
_________
You can also suspend a coroutine until a delegate returns false. 
The delegate is invoked each frame.

::

    IEnumerator WaitWhileRoutine()
    {
        // waits until space is released (assuming it's pressed right now)
        yield return BetterCoroutines.WaitWhile(() => Input.GetKey(KeyCode.Space));
    }

WaitForEndOfFrame
_________________
BetterCoroutines also has support for Unity's special WaitForEndOfFrame instruction. 

It'll suspend the coroutine until right before the current frame is displayed.

While you could create a new instance every time, there's no reason to punish the GC. Just use BetterCoroutines.WaitForEndOfFrame instead.

::

    IEnumerator EndOfFrameRoutine()
    {
        yield return BetterCoroutines.WaitForEndOfFrame;

        // take screenshot or whatever
    }

WaitUntil
_________
Similar to `WaitWhile`_ but instead it waits until the delegate returns true. 
The delegate is invoked each frame.

::

    IEnumerator WaitUntilRoutine()
    {
        // waits until Space is pressed.
        yield return BetterCoroutines.WaitUntil(() => Input.GetKeyDown(KeyCode.Space));
    }

WaitForWWW
__________
Yielding a WWW object will suspend the coroutine until the download is done.

::

    IEnumerator WaitForWWWRoutine()
    {
        WWWW www = new WWW("https://gitlab.com/archoninteractive/SwissArmyLib/raw/master/logo.png");
        
        yield return www;
        // or
        yield return BetterCoroutines.WaitForWWW(www);

        // www.texture is now downloaded 
    }

WaitForAsyncOperation
_____________________
Yielding an AsyncOperation will suspend the coroutine until that AsyncOperation is done.

::

    IEnumerator WaitForAsyncOperationRoutine()
    {
        AsyncOperation operation = SceneManager.LoadSceneAsync(0);
        
        yield return operation;
        // or
        yield return BetterCoroutines.WaitForAsyncOperation(operation);

        // scene is now loaded
    }

.. todo::
    Write some examples for BetterCoroutines