# Installation #

In order to use physaxe, you need to install the following :
  * [haXe](http://haxe.org/download) 1.19+
  * run `haxelib install physaxe` to install the physaxe package

# World, Body, Shape #

Using physaxe is pretty easy. First you have to create a `World` which is a virtual space for the simulation. A world can contain several `Body` instances and each body can contain several `Shape` instances.

Here's a haXe class that creates for example a world with three bodies and one floor :

```
class Test {

	static var world : phx.World;

	static function main() {
		// define the size of the world
		var size = new phx.col.AABB(-1000,-1000,1000,1000);
		// create the broadphase : this is the algorithm used to optimize collision detection
		var bf = new phx.col.SortedList();
		// initialize the world
		world = new phx.World(size,bf);
		// create one 50x50 box body at x=210,y=-50
		var b1 = new phx.Body(210,-50);
		b1.addShape( phx.Shape.makeBox(50,50) );
		// create one 30 radius circle at x=200,y=250
		var b2 = new phx.Body(200,250);
		b2.addShape( new phx.Circle(30,new phx.Vector(0,0)) );
		// create one 20x20 box body at x=100,y=270
		var b3 = new phx.Body(100,270);
		b3.addShape( phx.Shape.makeBox(20,20) );
		// add the created bodies to the world
		world.addBody(b1);
		world.addBody(b2);
		world.addBody(b3);
		// creates a 270x50 box at x=0,y=280
		var floor = phx.Shape.makeBox(270,50,0,280);
		// the floor is static, it can't move
		world.addStaticShape(floor);
		// setup gravity
		world.gravity = new phx.Vector(0,0.9);

		// for every frame, call the loop method
		flash.Lib.current.addEventListener(flash.events.Event.ENTER_FRAME,loop);
	}

	static function loop(_) {
		// update the world
		world.step(1,20);
		// clear the graphics
		var g = flash.Lib.current.graphics;
		g.clear();
		// draw the world
		var fd = new phx.FlashDraw(g);
		fd.drawCircleRotation = true;
		fd.drawWorld(world);
	}

}
```

You can compile this code with the following `test.hxml` file :

```
-swf test.swf
-swf-version 9
-swf-header 400:300:30
-main Test
-lib physaxe
```

The following shapes are available :

  * `phx.Polygon` : an arbitrary polygon - must be convex !
  * `phx.Shape.makeBox` : a shortcut to build a Box-polygon
  * `phx.Circle` : a circle with a radius and a vector defining the center of the circle
  * `phx.Segment` : a segment between two points with round borders and a given radius

# Stepping #

Each call to `world.step` will simulate the physics of the bodies :
  * the first parameter is the elapsed time that you want to simulate
  * the second parameter is the number of steps for the constraint solver. An higher number produce more accurate physics but slow down the simulation

After a call to `world.step`, the `Body` `x` and `y` fields contain the body position and the `a` field the body rotation (in radiants). You can then use these informations to update your graphics, for example set your MovieClip position to the virtual `Body` one.

You can also use `phx.FlashDraw` which can draw the whole world. Please notice that the whole world is redrawn everytime so this is quite slow if a lot of objects are involved.

# Customizing #

The following things can be customized :
  * the size of the world : regularly (every `world.boundsCheck` calls to `step`), all bodies are checked against world bounds. Bodies outside of the world are removed and their method `onDestroy` is called.
  * the gravity vector of the world : you can change it anytime, the way you want
  * each shape has a `material` field (an instance of `phx.Material`) which can be modified in order to set the density, friction and restitution. You can modify `phx.Const.DEFAULT_MATERIAL` if you need.
  * each body has a `properties` field (an instance of `phx.Properties`) which can be modified in order to set the friction, bias and max motion of the Body. You can modify `phx.Const.DEFAULT_PROPERTIES` if you need.
  * you can define your own broadphase by implementing the `phx.col.Broadphase` interface
  * after each call to `step`, each Body have its `arbiters` set to contain the contact points. You can use this information for specific collision handling/response.
  * you can create collision groups by modifying the `shape.groups`` integer. Two shapes can collide if `s1.groups & s2.groups != 0` (so the groups is used as bit list). By default, all groups are set to 1.
  * each Body and Shape have an unique ID, which you can use but please do not modify.

# AS3 Version #

If you are using AS3 and want to give physaxe a try, you can generate the AS3 sourcecode from the haXe source code by running the following commands :

```
haxe -lib physaxe -as3 phx -main Main --no-inline
mxmlc -output phx.swf phx/__main__.as
```

The first command will generate the AS3 sourcecode in the `phx` directory, the second one will compile the AS3 sourcecode as `phx.swf`.

Since the code is compiled with Adobe MXMLC, all haXe compiler optimizations are disabled and the resulting SWF speed will be reduced.

# Thanks #

Thanks to [Box2D](http://box2d.org) author Erin Catto for his great software and to Richard Jewson (Glaze) since I started working on Physaxe based on a port of Glaze to haXe, with heavy changes and optimizations.