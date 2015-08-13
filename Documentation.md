# Broadphases #

The broadphase is responsible of storing shapes in an optimized manner so we can reduce the number of shape-to-shape collisions tests.

There are currently several broadphases available in Physaxe :

  * `BruteForce` : simply store the shapes in a list and perform a check between all possible pair of shapes. This is of course very slow.
  * `SortedList` : sort shapes in a top-down ordered list. This enable to skip tests of shapes that don't overlap on the Y axis.
  * `Quantize` : divide world space into squares of `2^nbits` size. A shape will be stored into all squares its bounding box overlaps and only shapes within the same square will be tested. This is good for large worlds with small density.

You can also implement your own custom broadphase by implementing the `phx.col.Broadphase` interface (have a look at `BruteForce` implementation of an example).

# Moving a body #

If you want to manually change the position/rotation of a body, don't forget to call `world.sync(b)` after that in order to synchronize the body at the broadphase level.

# Islands & Sleeping #

An island is a group of touching bodies.

It has an energy which is calculated based on the `motion` of the bodies it contains. As soon as its energy is below `world.sleepEpsilon`, it will sleep and then no more collision between the bodies contained in the island will be performed.

A sleeping body will not be moved either.

You are not supposed to use islands directly, so sleeping is automatic (you can simply tune `sleepEpsilon` depending on your game). An island and its bodies are automatically wakeup if a another body gets connected to the island (in case an external collision occurs).

If you want to manually wakeup a body (because you manually applied to force to it) you can simply call `world.activate(b)`.