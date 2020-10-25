# FAQ's
Occasionally you may run into issues with GeckoLib. Before submitting an issue, please read through this page first.

#### Animation isn't playing

Check the console for logs, make sure GeckoLib is able to find the path to your animation file and the animation name's you are specifying are *exactly* what they are in the json file. Additionally, make sure you actually registered your AnimationController in `registerAnimationControllers`. Put a breakpoint in your AnimationPredicate and make sure it's actually being run.


#### Animation only plays once

Make sure that you set your animation to loop in blockbench. You can always use the overload of `AnimationBuilder#setAnimation()` that takes a loop parameter. This will override the loop value set in blockbench.


#### Animations take too long/too short to transition

Make sure your transition length is in ticks. You can change this value in the `AnimationController` constructor or by setting the field directly.


#### java.lang.NullPointerException: Unexpected error at net.minecraft.client.renderer.entity.EntityRendererManager.shouldRender

If you get this crash, make sure you're actually registering your entity renderer using `#!java RenderingRegistry.registerEntityRenderingHandler()`. If you've done this for your entity and you're still getting this crash, try removing any `@OnlyIn`'s or `@SideOnly`'s above your method.


If you still have problems, feel free to ask in our [discord](https://discord.com/invite/MNQcKxB) or make a github issue.