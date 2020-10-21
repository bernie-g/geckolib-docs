# Getting Started
!!! info
    This wiki presumes you have a basic understanding of Java, Minecraft, and Forge or Fabric. You'll need to understand how to register objects, read the Minecraft source code, and debug issues on your own. If you don't have these, consider watching [TurtyWurty's](https://www.youtube.com/watch?v=H55ClYTdQEI&list=PLaevjqy3XufYmltqo0eQusnkKVN7MpTUe) or [McJty's](https://wiki.mcjty.eu/modding/index.php?title=YouTube-Tutorials) tutorials.

## Creating a Model
To create a new GeckoLib Model, go to `File` -> `New` -> `GeckoLib Animated Model`

To choose your model type, go to `File` -> `GeckoLib Model Settings` and choose your object type.


![](https://i.softwarelocker.net/ojjRSn.gif)


## Converting an Existing Model
If you have already created a bedrock or java model, you can convert it to the GeckoLib format by going to `File` -> `Convert Project` -> Select `Geckolib Animated Model` Unfortunately there is no good way to convert a .java file into the GeckoLib format, since .java models are not lossless and are very tricky to import. In the future, make sure to save your `.bbmodel`'s

## Rigging
The process of preparing a model for animation is known as "rigging". You can think of it as the process of creating a skeleton for your model. Spending a little time rigging makes the animation process much easier.
### Grouping
Models consist of cubes, and groups. Only groups can be animated, so make sure to place all of your cubes in groups.

<img src="https://user-images.githubusercontent.com/110764/88623282-7b72a880-d059-11ea-9754-954428b481ab.png" width="300">

## Parenting and Pivots
The rig for a model is like a skeleton. Groups are the bones, pivots are the joints, and cubes are the flesh.
??? tip "MasterianoX Tip!"
    [![009-Parenting](https://user-images.githubusercontent.com/110764/88625203-22a50f00-d05d-11ea-9324-27bc82905482.png)](https://twitter.com/MasterianoX/status/1183125065250099200?s=19)
    These images are from the article [Minecraft Modeling & Texturing Tips by MasterianoX](https://blockbench.net/2019/10/02/minecraft-modeling-texturing-tips/). You can read it for more detail and lots more helpful modeling tips.
    
Unless your model has multiple object parts that can move independently (like if your entity was a school of fish), you probably want a single root group with many nested child groups. When each group moves, it also moves its children.



Pivot points can be set using the pivot tool and affect what point a group pivots from when it rotates.
??? tip "MasterianoX Tip!"
    [![016-Pivot-Points](https://user-images.githubusercontent.com/110764/88625350-6f88e580-d05d-11ea-8eb2-8cfdff740a88.png)](https://twitter.com/MasterianoX/status/1191089175681998848)
    These images are from the article [Minecraft Modeling & Texturing Tips by MasterianoX](https://blockbench.net/2019/10/02/minecraft-modeling-texturing-tips/). You can read it for more detail and lots more helpful modeling tips.

This is easier to explain visually so you can [watch this video showing how to set up parenting and pivots](https://eliot.s3.amazonaws.com/media/games/minecraft/blockbench/rigging.mov) for a simple skeleton:

[![Rigging Demo Video](https://user-images.githubusercontent.com/110764/88624641-3308ba00-d05c-11ea-8524-af9b720e8528.png)](https://eliot.s3.amazonaws.com/media/games/minecraft/blockbench/rigging.mov)



## Animating
You can animate your model in the Animation tab on the right. Geckolib currently supports position, scale, and rotation keyframes. Support for sound, particle, and custom event keyframes is in development. It's also important that you set the loop setting to the appropriate value for each animation in the editor. This will determine if the animation will loop in game. You can set this value by right-clicking the animation in the Animation Pane and selecting loop.


!!! info
    Animating in GeckoLib is almost exactly the same as animating a bedrock entity, so most bedrock animation tutorials also apply to GeckoLib.


## Working with easing curves
GeckoLib 2.0 added a powerful new feature called easing curves. These allow you to create smooth, natural-looking animations with less effort than previously.

In Bedrock and GeckoLib 1.0, the only type of "tweening" or interpolation that could be used between keyframes was linear interpolation, or "lerp". This means that as time progresses forwards, the value would change from the starting keyframe to the next keyframe at a constant speed. Real objects don't usually move in this way, they tend to need to accelerate a bit when starting and decelerate when stopping.

An easing curve is a mathematical function that can allow for animations to be interpolated in a more gradual fashion. The value of the "tween" at any given point in time is taken from the given easing curve rather than a straight line. This allows you to easily achieve effects like a smooth start and stop, an object overshooting its destination and sliding back into place (back curve), or an object bouncing (bounce curve).

??? tip "Easing Curve Demo"
    ![Easing Curves Animated Diagram](https://64.media.tumblr.com/5944bcf4f7fe9c0c99f7a593f233731a/tumblr_mj7bx09MDo1s5nl47o2_r1_500.gifv)

    _Source: [1ucasvb](https://1ucasvb.tumblr.com/post/44666043888/easing-functions-are-an-immensely-useful-tool-for). Note some curve names differ from ours but the principles are the same._

In addition, there are three directions a curve can be applied. "In" usually means the curve is applied focusing on the beginning of the interpolation, focusing on a smooth-looking start. "Out" usually means the curve is applied focusing on the end of the interpolation (the reverse of "In"), focusing on a smooth-looking end. "InOut" means the curve is symmetrically applied to both the start and end. In the animation above, when the animation plays from left to right it corresponds to "In", and when it plays backwards/right to left it corresponds to "Out". See below for a comparison:

![easing-directions](https://user-images.githubusercontent.com/110764/87970177-38706e00-ca78-11ea-8091-aa532e8c15ad.gif)

_Source: [Prototypr](https://prototypr.io/news/animation-easing-functions-explained/)_

We implemented all of the easing curves from [easings.net](https://easings.net/) and recommend you check out that website for an interactive, animated explanation of all the different curves and directions.

We also added a default "linear" curve to emulate bedrock behavior, and a "step" curve which snaps the value to a specified number of steps instead of moving smoothly. This can be used to animate things like clock hands or simulate a reduced framerate.

??? tip "Step Curve Demo"
    ![step-demo](https://user-images.githubusercontent.com/110764/87971479-43c49900-ca7a-11ea-9369-35acc5c350bf.gif)

In addition, we created arguments for the "back", "elastic", and "bounce" curves to give you additional control over their shapes.

In GeckoLib, we use the right-hand or "to" keyframe to specify the easing for each tween. Therefore, there is no easing curve on the first keyframe in any timeline. You need to **make a second keyframe in order to assign an easing curve**.

Eliot also made [a code sandbox to explore the different curves](https://editor.p5js.org/fadookie/present/rUp_FBuLn) and see the effect of the adjustment parameters for ones that have them. Feel free to play around with it if you find it helpful.

Here's a short demo of how to use the easing curves editor:
![easing-tutorial1](https://user-images.githubusercontent.com/110764/87967505-c39b3500-ca73-11ea-910c-65d72c772ccb.gif)


## Exporting Your Model
After you finish making your model in Blockbench, you need to export several things.

1. The actual model (`File` -> `Export` -> `GeckoLib Model`)
2. The texture (Right click the texture in the `Textures` panel and `Save As`)
3. Optionally the item display settings if you're in the Item mode. (`File` -> `Export` -> `GeckoLib Display Settings`)