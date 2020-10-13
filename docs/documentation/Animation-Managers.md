# Entity Animation Managers
Each entity should have exactly _one_ animation manager. There are a few things you can control with animation managers.

## Changing animation speeds
You can adjust how fast an entire entity is playing its animation using `EntityAnimationManager#setAnimationSpeed()`. Setting this value to 0 will make the animation freeze. Setting this value to 1 will make the animation play at default speed. Unfortunately, there's currently no way to adjust an individual AnimationController's speed, and you can only adjust the entire entity's speed.

## Changing reset speeds
If you return false in an `AnimationPredicate`, all the bones that were part of that animation will slowly move back to their original state defined in blockbench. If you want to change how long it takes for the bones to get back to their original state, use `EntityAnimationManager#setResetSpeedInTicks()`. 