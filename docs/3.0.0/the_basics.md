# Animation Basics
After you've got your animatable, model, and renderer class setup, you'll need to start figuring out when and how you want your animations to play. 
!!! note
    This page will be exactly the same for forge and fabric as no Minecraft code will be referenced.
    
## IAnimatable's
The `IAnimatable` interface represents any object that can have control animations. It doesn't necessarily need to be a concrete Minecraft object like a block as seen in the replacing entities article. 

The two methods you need to override in IAnimatable are as follows:

- `#!java getFactory()` - All you need to here is return a member field of type `AnimationFactory`. This needs to be passed in an instance of your `IAnimatable` (usually done by passing in `this`)
- `#!java registerControllers()` - This method is called everytime a new instance of your animatable object (in this case your entity) is created. So, you should create your animation controllers, register listeners, and do any initial setup here. Take a look at the example below for a more obvious example. Although not recommended, you can store a reference to any animation controllers created in this method **only for non-singleton objects like entities and tile entities**.

## Animation Factories
An AnimationFactory should be created once for every IAnimatable instance. Usually this is declared as a member field. The only time you will need to use this is to return it in the `#!java getFactory()` method and in special circumstances like items.

??? note
    In reality, an AnimationFactory is just a hashmap where the key is a unique ID that identifies a certain animatable instance and the value is the AnimationData that correlates to the animatable instance.
    
## AnimationData
AnimationData serves several purposes. It stores all the animation controllers for that specific animatable instance, along with some extra private info you don't need to worry about. The main thing you need to do with an AnimationData variable is register AnimationControllers and retrieve them when needed.

To register an AnimationController to an AnimationData object, simply call `#!java addAnimationController()`;

## Animation Controllers
Animation Controllers are the core of how GeckoLib control's animations. Each controller can have exactly *one* animation playing at a time. To play multiple animations at the same time, simply create and register multiple `AnimationController`'s.

An animation controller fundamentally has 3 possible states. You can find out what state a controller is in by checking `#!java AnimationController.getAnimationState()`

| State         | Explanation                                                                                                             |
|---------------|-------------------------------------------------------------------------------------------------------------------------|
| Running       | Indicates that the controller is actively playing an animation                                                          |
| Transitioning | Controller is transitioning from stopped to running or from one animation to another                                    |
| Stopped       | Controller is not actively running an animation. Either completely still or lerping back to the model's original state. |

### Animation Manipulation
To find out what animation a controller is currently playing, check `#!java AnimationController.getCurrentAnimation()`. If this is null, it means that either the controller is stopped or it has not begun playing the animation yet. 

To switch a controller's currently playing, simply call `#!java setAnimation(builder)` with an appropriate AnimationBuilder. This method is effectively cached, meaning that if you call it multiple times with the same AnimationBuilder it will have no effect. If this is not desired behavior, simply call `#!java AnimationController.markNeedsReload()`. This will clear the current cached `AnimationBuilder` and the controller will restart the animation when you call `#!java setAnimation()`. If this is confusing to you, look at the `JackInTheBoxItem` example.

### Creating an Animation Controller
You should almost always create your animation controllers inside the `#!java registerControllers()` method. Simply use the controller constructor and pass in `this`, the name of the controller, the transition length in ticks, and a reference to your animation predicate method. Make sure you remember to add the controller to the `AnimationData`.

### Animation Predicates
When you create a controller, you have to pass in a method reference to a predicate that will get called every render frame. Most of the time you set your animations in this method. The animation predicate has a return type of `PlayState`. Return `PlayState.CONTINUE` if you want the controller to keep playing the current animation or `PlayState.STOP` if you want it to stop.

### Transitioning
By default, all animations in GeckoLib are transitioned to/from. When you start an animation for the first time, GeckoLib will figure out how to linearly interpolate from its current position to the start of the next animation. Moreover, when you stop an animation, the model will transition back to it's original state. You can change how long it takes to transition in the `AnimationController` constructor, or simply changing `#!java AnimationController.transitionLengthTicks`. Keep in mind this value is in ticks, not seconds. To disable transitioning altogether and simply snap between animations, just set this value to 0.

### Overriding Easing
As explained in the getting started article, all GeckoLib keyframes have an easing type. However, sometimes you may want to override these and change them in code. You can do this for an entire animation controller, by setting `#!java AnimationController.easingType` to one of the `EasingType` enum values, such as `EaseInSine` or `EaseInOutQuad`. Setting it to `Linear` will disable easing completely, overriding all keyframe easing types. `Custom` will allow you to use your own custom easing function, and `None` will disable the feature completely, defaulting back to keyframe easings.

#### Custom Easing Methods
GeckoLib allows you to provide your own easing function as well as use the presets. Your function should be of type `Function<Double, Double>`, where the input is a number between 0 to 1 indicating how far in a keyframe you are and the output is the "eased" number between 0 and 1. To read more about how this works and for a better explanation, look at [easings.net](https://easings.net). Once you've created your function, simply assign it to `#!java AnimationController.customEasingMethod` and set `#!java AnimationController.easingType` to `#!java EasingType.CUSTOM`.

