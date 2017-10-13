Grid3D<T>
=========

.. highlight:: csharp

A fixed (but resizable) 3D grid of items. 

Usage
-----
Creation
~~~~~~~~
::

    // creates a 100x100x100 grid
    Grid3D<float> myGrid = new Grid3D<float>(100, 100, 100);

You can also define what value cells should have by default::

    Grid3D<float> myGrid = new Grid3D<float>(100, 100, 100, 1f);

Changing a cell
~~~~~~~~~~~~~~~
::

    myGrid[3, 10, 30] = 128f;

    // or

    myGrid.Set(3, 10, 30, 128f);

Fill
~~~~
You can fill everything within a cube with *Fill()*.

::

    int minX = 30, minY = 40, minZ = 23;
    int maxX = 50, maxY = 60, maxZ = 80;
    myGrid.Fill(128f, minX, minY, minZ, maxX, maxY, maxZ);

Clear
~~~~~
You can clear the whole grid with a specific value by using *Clear()*.

::

    // sets every cell to the default value
    myGrid.Clear();

    // sets every cell to 128f
    myGrid.Clear(128f);

Resize
~~~~~~
If the size of the grid suddenly doesn't cut it you can always resize it.

The cell contents are kept the same and any new cells are initialized with the *DefaultValue*.

Growing the grid will allocate new arrays, shrinking will not.

::

    myGrid.Resize(300, 100, 150);

.. todo::
    Write some examples for Grid2D