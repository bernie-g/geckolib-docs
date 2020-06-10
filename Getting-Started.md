# Getting Started
## Installing

To use the library in a dev environment, add this to your build.gradle file.
```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.bernie-g:geckolib:1.0.0-1.15'
}
```

## Installing the Plugin
In order to use blockbench (bedrock) animations in forge, you'll need to install the Geckolib blockbench plugin. You can find it by going to File -> Plugins -> Available -> Search for _"Gecko's Animation Utils"_

![blockbench plugin](https://i.softwarelocker.net/CIPrU7.png)

## Creating a Model
In order to create a model compatible with Geckolib, you should create a new Animated Entity Model in blockbench, by going to File -> New Animated Java Entity.

![](https://i.softwarelocker.net/CQ3git.png)

## Converting an Existing Model
If you have already created a bedrock or normal java entity, you can convert it to an Animated Entity Model compatible with Geckolib by going to File -> Convert Project -> Select _"Animated Java Entity"_. Keep in mind that if you're converting from a bedrock model to a java model, there are occasionally hiccups in the model conversion process since the models are stored in different file formats. You'll have to either fix these issues manually or consult the blockbench discord.

## Animating
You can animate your model in the Animation tab on the right. Geckolib currently supports position, scale, and rotation keyframes. Support for sound, particle, and custom event keyframes is in development. It's also important that you set the loop setting to the appropriate value for each animation in the editor. This will determine if the animation will loop in game. You can set this value by right-clicking the animation in the Animation Pane and selecting loop.

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

## Exporting Your Model
After you finish making your model in Blockbench, you need to export both the .java entity model and the .json animation file. You can export the model by going to File -> Export -> Export Animated Java Entity. You can export the animation json file by going to Animation -> Export Animations

### Read Next
To add your model and animation in game, read how to do so [here.](https://github.com/bernie-g/geckolib/wiki/Using-Your-Animations)