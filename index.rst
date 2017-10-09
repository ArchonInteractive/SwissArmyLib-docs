SwissArmyLib
========================================

.. toctree::
    :hidden:

    Getting Started
    Coroutines/BetterCoroutines
    Events/index
    Pooling/index
    Automata/index
    Partitioning/index
    Resource System/index
    Collections/index
    Utils/index

.. image:: https://ci.appveyor.com/api/projects/status/sapkbwkbl5ug901u/branch/master?svg=true
    :alt: Build status
    :target: https://ci.appveyor.com/project/Phault/swissarmylib/branch/master

.. image:: https://readthedocs.org/projects/swissarmylib-docs/badge/?version=latest
    :alt: Documentation Status
    :target: http://swissarmylib-docs.readthedocs.io/en/latest/?badge=latest

.. image:: https://api.bintray.com/packages/phault/SwissArmyLib/development/images/download.svg
    :alt: Download
    :target: https://bintray.com/phault/SwissArmyLib/development/_latestVersion#files

`Repository <https://github.com/ArchonInteractive/SwissArmyLib>`_
•
`API Reference <https://archoninteractive.com/swissarmylib/>`_

----

About
-----

**SwissArmyLib** is an attempt to create a collection of useful utilities with a focus on being performant. It is primarily intended for Unity projects, but feel free to rip parts out and use them for whatever you want.

A very important part in the design decisions of this library was to keep the garbage generation low. This means you will probably frown a little upon the use of interfaces for callbacks, instead of just using delicious delegates. It also means using the trusty old *for* loops for iterating through collections where possible.

There's a lot of libraries with some of the same features, but they're often walled off behind a restrictive or ambiguous license.
This project is under the very permissive MIT license and we honestly do not care what you use it for.

Features
--------

* :doc:`Events <Events/Event>`
    * Supports both interface and delegate listeners
    * Can be prioritized to control call order
    * Check out :doc:`GlobalEvents <Events/GlobalEvents>` if you need.. well.. global events.
* :doc:`Timers <Events/TellMeWhen>`
    * Supports both scaled and unscaled time
    * Optional arbitrary args to pass in
    * Also uses interfaces for callbacks to avoid garbage
* :doc:`Coroutines <Coroutines/BetterCoroutines>`
    * More performant alternative to Unity's coroutines with a very similar API.
* :doc:`Automata <Automata/index>`
    * :doc:`Finite State Machine <Automata/Finite State Machine>`
    * :doc:`Pushdown Automaton <Automata/Pushdown Automaton>`
* :doc:`Pooling <Pooling/index>`
    * Support for both arbitrary classes and GameObjects
    * :doc:`IPoolable <Pooling/IPoolable>` interface for callbacks
        * :doc:`PoolableGroup <Pooling/PoolableGroup>` component in case multiple IPoolable components needs to be notified
    * Timed despawns
* :doc:`Service Locator <Utils/Service Locator>`
    * An implementation of the Service Locator pattern
    * Aware of MonoBehaviours and how to work with them
    * Supports scene-specific resolvers
    * Supports both singletons and short-lived objects
        * Singletons can be lazy loaded
* :doc:`Managed Update Loop <Events/ManagedUpdate>`
    * An update loop maintained in managed space to avoid the `overhead of Native C++ --> Managed C# <https://blogs.unity3d.com/2015/12/23/1k-update-calls/>`_
    * Useful for non-MonoBehaviours that needs to be part of the update loop
    * Optional :doc:`ManagedUpdateBehaviour <Events/ManagedUpdateBehaviour>` class for easy usage
* :doc:`Spatial Partitioning <Partitioning/index>`
    * GC-friendly implementations of common space-partitioning systems
* :doc:`Resource Pool <Resource System/index>`
    * Generic and flexible resource pool (health, mana, energy etc.)
* Gravity
    * Flexible gravitational system
    * Useful for planet gravity, black holes, magnets and all that sort of stuff.
* Misc
    * :doc:`BetterTime <Utils/BetterTime>`
        * A wrapper for Unity's static Time class that caches the values per frame to avoid the marshal overhead.
        * About 4x faster than using the Time class directly, but we're talking miniscule differences here.
    * Shake
        * Useful for creating proper screen shake
    * :doc:`Some collection types <Collections/index>`
    * :doc:`Some useful attributes <Utils/Attributes/index>`
        * :doc:`ExecutionOrder <Utils/Attributes/ExecutionOrder>`
            * Sets a default (or forces) an execution order for a MonoBehaviour
        * :doc:`ReadOnly <Utils/Attributes/ReadOnly>`
            * Makes fields uninteractable in the inspector
    * A few other tiny utilities

Download
~~~~~~~~
Binaries for the bleeding edge can be found `here <https://bintray.com/phault/SwissArmyLib/development/_latestVersion#files>`_.
Alternatively you can either :ref:`build it yourself <gettingstarted-source>` (very easily) or simply :ref:`copy the source code into your Unity project <gettingstarted-copysource>` and call it a day.

License
~~~~~~~
`MIT <https://tldrlegal.com/license/mit-license>`_ - Do whatever you want. :) 

Contributing
~~~~~~~~~~~~
Pull requests are very welcome!

I might deny new features if they're too niche though, but it's still very much appreciated!
