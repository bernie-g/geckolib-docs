# Effect Keyframes
GeckoLib supports more than just model animation. You can add sound keyframes to trigger sounds at specific points in an animation, particles to display effects, and custom instruction keyframes to add your own custom event data to animations.

To enable this feature in blockbench, click **Animation -> Animate Effects**. You'll see a new animation panel pop up in the animator. From here, you can add global keyframes to your model.

## Sound Keyframes
GeckoLib handles sounds automatically. All you have to do is subscribe to the sound listener using `AnimationController#registerSoundListener()` and return your SoundEvent based on the event's data. You can see an example of a sound listener in [the tigris entity.](https://github.com/bernie-g/geckolib/blob/1.15/src/main/java/software/bernie/geckolib/example/entity/TigrisEntity.java#L52)

## Particle Keyframes 
GeckoLib doesn't do any extra work for particles. However, you can still subscribe to the particle listener and do your own particle rendering if you'd like. This process is exactly the same as for sound keyframes. (If you actually use this feature and do something cool with it I'd love to see it in action). You can see an example of a particle listener in [the easing demo entity.](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/geckolib/example/entity/EasingDemoEntity.java#L36)

## Custom Instruction Keyframes
Custom instructions are useful for non-sound and non-particle things you want to do to your entity at a specific time in your keyframe. This can be anything, from rendering a hat, to sending a chat message. You can see an example of a custom instruction listener in [the easing demo entity.](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/geckolib/example/entity/EasingDemoEntity.java#L37)
