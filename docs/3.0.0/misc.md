# Miscellaneous
This wiki page will be full of miscellaneous topics that don't really fit into any other category and aren't large enough to have their own page.

## Hot Reloading Models
Since GeckoLib models are all resource-packable, you can easily reload models and animations by pressing:

++f3+t++

This will reload textures, models, and animations instantly. If you are in a dev environment, make sure you are actually recompiling your edited files. In intellij this can be done by right clicking the file and clicking `Recompile File`.

## Combining Code Animations with GeckoLib
Sometimes you want to have an animated model, but also move some bones using code. An example use-case would be making an entity look at the player and animate at the same time. Although this can be accomplished with molang, you can also override `setLivingAnimations` and set your bone rotations after the super call. You can also do this in the `renderEarly` method in your renderer class.

!!! note
    Note that if you remove the super call in `setLivingAnimations` the model will no longer animate at all. 

To get a bone by name, simply call `#!java this.getAnimationProcessor().getBone("leftArm")` in your model class. You can then rotate/translate/scale this bone however you wish.

## Resource Packs
All GeckoLib models and animations can be overriden in resource packs. This opens up a ton of possibilities, since before GeckoLib it was impossible to override models/animations for entities, much less animations. To create a resource pack that overrides geckolib models/animations, simply do everything how you would with a normal resource pack, and place your models in `/geo` and animations in `/animations`. That's it! Just press F3+T and your changes will instantly be reloaded in game.

