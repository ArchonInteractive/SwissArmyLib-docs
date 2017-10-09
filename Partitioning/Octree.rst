Octree<T>
=========

.. highlight:: csharp

A GC-friendly `Octree <https://en.wikipedia.org/wiki/Octree>`_ implementation.

Use the static *Create()* and *Destroy()* methods for creating and destroying trees. If you forget to *Destroy()* a tree when you're done with it, it will instead be collected by the GC as normal, but the nodes will not be recycled.

If you're doing 2D, take a look at :doc:`Quadtree&lt;T&gt; <Quadtree>`.

Usage
-----
To be done.

Examples
--------
To be done.