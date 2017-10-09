BetterTime
==========

.. highlight:: csharp

BetterTime is just a wrapper for Unity's Time class. The thing with their class is that every call has a marshal overhead since it calls their native C++ code. Since most of the properties don't change as often as they're called, it is worthwhile to cache them. This is what BetterTime does in the background.

It is a small performance benefit, but it is totally free as it's just about writing **BetterTime** instead of **Time** and everything remains the same.

Usage
-----
Every UnityEngine.Time property is wrapped and the API stays the same. Just call BetterTime instead of Time.

Eg. instead of::

    transform.position += velocity * Time.deltaTime;

You should write::

    transform.position += velocity * BetterTime.DeltaTime;