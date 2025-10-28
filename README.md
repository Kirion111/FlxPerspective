
# IMPORTANT
This is a modified version of the
original FlxPerspective library made by [TheZoroForce240](https://github.com/TheZoroForce240) to use in Haxe 4.3.5

Go check [FlxPerspective](https://github.com/TheZoroForce240/FlxPerspective) Made By [TheZoroForce240](https://github.com/TheZoroForce240)
(Really cool dude)

# FlxPerspective
3D in [HaxeFlixel](https://github.com/HaxeFlixel/flixel) by using a modified Vertex shader to transform sprites and triangles into a 3D space.

![](https://github.com/TheZoroForce240/FlxPerspective/blob/main/examples/loop.gif)

## Features
* FlxPerspectiveSprites that can change the Z value and offset the corners to skew the sprite
* A 3D Camera system that can move and look around the environment (with some built-in debug controls)
* FlxPerspectiveStrips to render triangles and support for loading in 3D models (.obj files)

## Limitations
* No depth testing right now! Sprites will need to be layered manually or will need to be added to a FlxScene3D that attempts to auto layer based on the camera position, its not perfect but its the only option right now.
* Flixel's culling system needed to be turned off for this to work! Frustum culling will hopefully be implemented at some point in the future to fix this.
* A few edited Flixel files are needed to correctly render 3D models with FlxPerspectiveStrip, this is to enable triangle culling and disable the camera boundary check.

## Installation
* Run this command to install the haxelib
```
haxelib git flxperspective https://github.com/Kirion111/FlxPerspective
```
* Add "flxperspective" to the project.xml
```xml
<haxelib name="flxperspective" />
```

## Example usage

FlxPerspectiveSprite
```haxe
var scene = new FlxScene3D();
add(scene);
                                   //x, y, z
var spr = new FlxPerspectiveSprite(-200, 0, 200);
spr.makeGraphic(200, 200, FlxColor.BLUE);
scene.add(spr);
```

FlxPerspectiveStrip with a 3D Model
```haxe

FlxG.cameras.reset(new FlxCameraPerspective()); //required for 3D models to look right

var sphere = new FlxPerspectiveStrip(0, 600, 0);
sphere.repeat = true;
sphere.loadGraphic(FlxGraphic.fromClass(GraphicLogo));
sphere.applyModelData(OBJLoader.loadFromAssets("assets/models/sphere.obj")[0]); //index 0 because its using a single material
scene.add(sphere);

//example of multi-textured model
var modelData = OBJLoader.loadFromAssets("assets/models/coolmodel.obj");
for (md in modelData)
{
  var piece = new FlxPerspectiveStrip(0, 0, 0);
  piece.repeat = true;
  piece.loadGraphic("assets/models/" + md.mtl.diffuseTexture);
  piece.applyModelData(md, true, true);
  scene.add(piece);
}
```
