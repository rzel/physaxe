Physaxe is a 2D Rigid Body Library written in Haxe. It's been highly optimized for the Flash 9 Player, with the best optimizations available.

Physaxe is based on several existing physics engines, mainly :
  * [Box2D](http://box2d.org), the reference open source physics engine
  * Glaze, an AS3 engine which is a port of [Chipmunk](http://wiki.slembcke.net/main/published/Chipmunk), itself based on Box2D

Physaxe features are :
  * rigid body consisting in several shapes
  * shapes supported are circles, segments (with rounded edges) and arbitrary convex polygons
  * customizable broadphase (currently bruteforce and y-sorted list are available)
  * island resolution and sleeping (allow ~0 CPU to be spent when groups are sleeping)
  * constraint solver based on Box2D sequential impulses
  * customizable body properties, such as linear/angular friction and maximized motion

What to give it a try ? Read the [Tutorial](http://code.google.com/p/physaxe/wiki/Tutorial)

And watch the [Demo](http://ncannasse.fr/blog/physaxe)