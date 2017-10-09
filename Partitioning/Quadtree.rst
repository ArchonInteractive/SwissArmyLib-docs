Quadtree<T>
===========

.. highlight:: csharp

A GC-friendly `Quadtree <https://en.wikipedia.org/wiki/Quadtree>`_ implementation.

Use the static *Create()* and *Destroy()* methods for creating and destroying trees. If you forget to *Destroy()* a tree when you're done with it, it will instead be collected by the GC as normal, but the nodes will not be recycled.

If you're doing 3D, take a look at :doc:`Octree&lt;T&gt; <Octree>`.

Usage
-----
To be done.

Examples
--------
To be done.