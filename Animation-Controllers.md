# Animation Controllers
GeckoLib is able to mix and match animations, play multiple simultaneously, and smoothly transition between animations. Animation Controllers are what make this possible. Each Animation Controller should represent a logical animation category. For instance, if you want your entity to have a walking animation, an arm animation, and a head animation, you should split it up into as many Animation Controllers as possible. Although this may seem counter intuitive, you give yourself much more control by being able to mix and match animations whenever you want.

### Transition Lengths
Each Animation Controller has a transition length property, which determines how many **ticks** it takes to transition from one animation to another. You can also change this property during an `AnimationPredicate`, to dynamically change how long it takes to transition between animations.

## Limitations

Even though GeckoLib allows you to play multiple animations at the same time, you must take care to never play two Animations that involve the same bone properties at the same time. For example, say you have two bones, Bone A and Bone B.

You **CAN**:
* Play two unrelated animations, one on each bone
* Play one animation that changes Bone A's rotation and Bone B's Position
* Play one animation that changes Bone A's rotation, and a different animation that changes Bone A's position/scale

You **CANNOT**:
* Play one animation that changes Bone A's position, and a different animation that also changes Bone A's position


Each Animation Controller can only play one animation at a time, however you can assign entire queues of animations to an Animation Controller, so you can play one Animation after another, after another, after another, etc. This can be accomplished using Animation Builders.

## Animation Builders
Animation Builders follow the builder pattern. This means that every method returns an instance of this class. You can stack method calls, like this: 
```java
AnimationBuilder jumpAnimationBuilder = new AnimationBuilder().addAnimation("jump").addRepeatingAnimation("run", 5");
jumpController.setAnimation(jumpAnimationBuilder);
```

`AnimationBuilder` exposes several methods to set animations.

1. `addAnimation(String animationName)` - Adds a single animation to the builder
2. `addAnimation(String animationName, Boolean shouldLoop)` - Adds a single animation and overrides the loop setting on that animation.
3. `addRepeatingAnimation(String animationName, int timesToRepeat)` - Adds a single animation to the queue several times. This is useful if you want to run a single animation several times, and then run a different animation afterwards. You can of course just run `addAnimation` several times.

After creating your `AnimationBuilder`, you need to assign it to the `AnimationController`. You can do this with `AnimationController#setAnimation(AnimationBuilder builder)`. You should run this client side, and usually in the AnimationPredicate. This method _can be run every tick, and the animation won't be restarted every tick_. It's also important that the name of the animation you set is exactly the same as in the json file. **Sometimes the name of the animation in the json file is not the same as in blockbench**

Full Example of using an `AnimationBuilder` inside an `AnimationPredicate`:

```java
private <ENTITY extends Entity> boolean moveController(AnimationTestEvent<ENTITY> entityAnimationTestEvent)
	{
		moveController.transitionLength = 10;
		if(KeyboardHandler.isQDown)
		{
			moveController.setAnimation(new AnimationBuilder().addAnimation("tigris.spitfly", false).addAnimation("tigris.sit", false).addAnimation("tigris.sit", false).addAnimation("tigris.run", false).addAnimation("tigris.run", false).addAnimation("tigris.sleep", true));
		}
		else {
			moveController.setAnimation(new AnimationBuilder().addAnimation("tigris.fly"));
		}
		return true;
	}
```