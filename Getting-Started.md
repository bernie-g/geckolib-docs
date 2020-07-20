# Getting Started

## Table of Contents

   * [Getting Started](#getting-started)
      * [Installing](#installing)
      * [Installing the Plugin](#installing-the-plugin)
      * [Creating a Model](#creating-a-model)
      * [Converting an Existing Model](#converting-an-existing-model)
      * [Animating](#animating)
      * [Animation Concepts](#animation-concepts)
      * [(2.0) Working with easing curves](#20-working-with-easing-curves)
      * [Exporting Your Model](#exporting-your-model)
         * [(2.0) Animated Entity Settings Window](#20-animated-entity-settings-window)
         * [Read Next](#read-next)

## Installing

To use the library in a dev environment, add this to your build.gradle file. For forge users, you _have to make a new repositories block_. Otherwise, your build will fail. **Do not add the jitpack repo to the buildscript section of your gradle.**



For 1.15.2:
```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation fg.deobf('com.github.bernie-g:geckolib:1.15-1.0.2')
}
```

For 1.15.2 Fabric:
```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation fg.deobf('com.github.bernie-g:geckolib:fabric-1.15-1.0.0')
}
```

For 1.12.2:
```gradle
minecraft {
    useDepAts = true
}

repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.bernie-g:geckolib:1.0.0-1.12'
}
```

## Installing the Plugin
In order to use blockbench (bedrock) animations in forge, you'll need to install the Geckolib blockbench plugin. You can find it by going to File -> Plugins -> Available -> Search for _"GeckoLib Animation Utils"_

![blockbench plugin](https://i.softwarelocker.net/CIPrU7.png)

## Creating a Model
In order to create a model compatible with Geckolib, you should create a new Animated Entity Model in blockbench, by going to File -> New Animated Java Entity.

![](https://i.softwarelocker.net/CQ3git.png)

## Converting an Existing Model
If you have already created a bedrock or normal java entity, you can convert it to an Animated Entity Model compatible with Geckolib by going to File -> Convert Project -> Select _"Animated Java Entity"_. Keep in mind that if you're converting from a bedrock model to a java model, there are occasionally hiccups in the model conversion process since the models are stored in different file formats. You'll have to either fix these issues manually or consult the blockbench discord.

## Animating
You can animate your model in the Animation tab on the right. Geckolib currently supports position, scale, and rotation keyframes. Support for sound, particle, and custom event keyframes is in development. It's also important that you set the loop setting to the appropriate value for each animation in the editor. This will determine if the animation will loop in game. You can set this value by right-clicking the animation in the Animation Pane and selecting loop.

Animating in GeckoLib is almost exactly the same as how you would animate for a bedrock entity. There are currently no GeckoLib specific video tutorials, but you can watch this bedrock animation [tutorial](https://www.youtube.com/watch?v=eVYCXVzkp1Q) instead.

## Animation Concepts
Ideally, animations should be split up as much as possible. Geckolib allows you to run multiple animations simultaneously, so in order to make the smoothest transitions, you should split up each logical animation. For example, if you're making a flying creature with several flying types, several running types, and several head movements, you should split each one into it's own animation and combine them in code. 

In this example, you should make these animations in blockbench (examples):
* Default Head Movement Animation
* Spitting Head Animation
* Wing Flapping Animation (only involving wings)
* Faster Wing Flapping Animation (only involving wings)
* Running Animation (only involving legs)
* Walking Animation (only involving legs)

This is different to the normal way most people animate. Usually, you would animate the entire body at once and duplicate it + adjust keyframes. This can certainly work, but it will provide for a less seamless transition period in between animations. Additionally, the modular system Geckolib encourages allows for more possible animation combinations and a greater control for the developer.

## (2.0) Working with easing curves
GeckoLib 2.0 added a powerful new feature called easing curves. These allow you to create smooth, natural-looking animations with less effort than previously.

In Bedrock and GeckoLib 1.0, the only type of "tweening" or interpolation that could be used between keyframes was linear interpolation, or "lerp". This means that as time progresses forwards, the value would change from the starting keyframe to the next keyframe at a constant speed. Real objects don't usually move in this way, they tend to need to accelerate a bit when starting and decelerate when stopping.

An easing curve is a mathematical function that can allow for animations to be interpolated in a more gradual fashion. The value of the "tween" at any given point in time is taken from the given easing curve rather than a straight line. This allows you to easily achieve effects like a smooth start and stop, an object overshooting its destination and sliding back into place (back curve), or an object bouncing (bounce curve).

![Easing Curves Animated Diagram](https://64.media.tumblr.com/5944bcf4f7fe9c0c99f7a593f233731a/tumblr_mj7bx09MDo1s5nl47o2_r1_500.gifv)

_Source: [1ucasvb](https://1ucasvb.tumblr.com/post/44666043888/easing-functions-are-an-immensely-useful-tool-for). Note some curve names differ from ours but the principles are the same._

In addition, there are three directions a curve can be applied. "In" usually means the curve is applied focusing on the beginning of the interpolation, focusing on a smooth-looking start. "Out" usually means the curve is applied focusing on the end of the interpolation (the reverse of "In"), focusing on a smooth-looking end. "InOut" means the curve is symmetrically applied to both the start and end. In the animation above, when the animation plays from left to right it corresponds to "In", and when it plays backwards/right to left it corresponds to "Out". See below for a comparison:

![easing-directions](https://user-images.githubusercontent.com/110764/87970177-38706e00-ca78-11ea-8091-aa532e8c15ad.gif)

_Source: [Prototypr](https://prototypr.io/news/animation-easing-functions-explained/)_

We implemented all of the easing curves from [easings.net](https://easings.net/) and recommend you check out that website for an interactive, animated explanation of all the different curves and directions.

We also added a default "linear" curve to emulate bedrock behavior, and a "step" curve which snaps the value to a specified number of steps instead of moving smoothly. This can be used to animate things like clock hands or simulate a reduced framerate.

In addition, we created arguments for the "back", "elastic", and "bounce" curves to give you additional control over their shapes.

In GeckoLib, we use the right-hand or "to" keyframe to specify the easing for each tween. Therefore, there is no easing curve on the first keyframe in any timeline. You need to **make a second keyframe in order to assign an easing curve**.

Eliot also made [a code sandbox to explore the different curves](https://editor.p5js.org/fadookie/present/rUp_FBuLn) and see the effect of the adjustment parameters for ones that have them. Feel free to play around with it if you find it helpful.

Here's a short demo of how to use the easing curves editor:
![easing-tutorial1](https://user-images.githubusercontent.com/110764/87967505-c39b3500-ca73-11ea-910c-65d72c772ccb.gif)


## Exporting Your Model
After you finish making your model in Blockbench, you need to export both the .java entity model and the .json animation file. You can export the model by going to File -> Export -> Export Animated Java Entity. You can export the animation json file by going to Animation -> Export Animations

### (2.0) Animated Entity Settings Window
GeckoLib 2.0 added an `Animated Entity Settings` Window which can be accessed from the `File` menu for Animated Java Entity projects:

<img width="231" alt="Screen Shot 2020-07-20 at 10 38 53 AM" src="https://user-images.githubusercontent.com/110764/87968463-4ffa2780-ca75-11ea-9f41-a8320c3efd3e.png">

This can be used to customize the Java code template so that you can export many times if needed without having to manually edit the code afterwards. These settings are completely optional, you can just edit the code by hand still if you prefer.

<img width="550" alt="Screen Shot 2020-07-20 at 9 39 56 AM" src="https://user-images.githubusercontent.com/110764/87968351-22ad7980-ca75-11ea-9ae3-2abb49ae0cdf.png">

| Setting                  | Description                                                                                                                                                                           |
| ---                      | ---                                                                                                                                                                                   |
| Modding SDK              | Choose which Modding SDK and version you are using so code will be generated in the correct format.                                                                                   |
| Entity Type              | The fully qualified type name of your Entity which will be supplied as the type parameter to the model's superclass, `AnimatedEntityModel`, ex. `com.example.mymod.entities.MyEntity` |
| Java Package             | The Java package you want your model to be in, ex. `com.example.mymod.models`                                                                                                         |
| Animation File Namespace | The namespace where your animation file resource is in. This should probably be your mod ID, ex. `mymod`.                                                                             |
| Animation File Path      | The path to the animation file inside the namespace, ex. `animations/my_animation.json`.                                                                                              |


### Read Next
To add your model and animation in game, read how to do so [here.](https://github.com/bernie-g/geckolib/wiki/Using-Your-Animations)