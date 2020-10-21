# Overriding Animations
There are several instances where you'll want to override parts of an animation completely. GeckoLib allows you to change several parts of your animation at runtime.

## Overriding Looping
There are three places you can change this setting. Listed in priority of highest to lowest:
1. `AnimationController#loopByDefault`
2. `animationBuilder.addAnimation("animation", true)` where true represents that the animation should loop
3. `"loop" = true` in the animation json file. This can be configured in blockbench by right clicking your animation and selecting loop.

This image explains how GeckoLib determines if an animation should loop.
<img src="https://i.softwarelocker.net/Untitled%20Document%281%29.png" width="800" height="800">


## Overriding Easing
By default, GeckoLib takes the easing type from each keyframe in the json file and applies them individually. However, you can also override this functionality for an entire `AnimationController` by setting it's `easingType` to one of the `EasingType` enums. To disable this overriding behavior, set it to `EasingType.NONE`.

### Custom Easing Curves
If for some reason you want to make a completely new easing curve that isn't included in blockbench, GeckoLib allows you to do this. This can be accomplished by setting `AnimationController#easingType = EasingType.CUSTOM`, and registering your custom easing function to `AnimationController#customEasingMethod = this::easingMethod`. This function takes a decimal between 0 to 1 as input and returns another decimal representing the lerp value the program should use. Take a look at `EasingManager` for more info.

# Hot Reloading
GeckoLib provides a simple command to hot swap animation files. Simply change your animation file, rebuild your project in your IDE, and execute `/geckolib reload`

# Sounds
GeckoLib has [support ](Sounds,-Particles,-and-Custom-Instruction-Keyframes#sound-keyframes) for sound keyframes from blockbench. To adjust the volume, pitch, and distance delay of the sounds played, adjust the fields in your AnimationController.