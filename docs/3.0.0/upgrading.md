# Upgrading from GeckoLib 2.0.0

The biggest difference when upgrading to 3.0.0 is that legacy `.java` are no longer supported. You **must** use `.geo.json` bedrock models, which means you'll need to convert your existing models. 

## Small Changes

- `IAnimatedEntity` is now `IAnimatable`
- `EntityAnimationController` is now `AnimationController`
- `EntityAnimationManager` is now `AnimationFactory`, but it works differently and the constructor takes an `IAnimatable`, which you usually pass in using `this`
- `getAnimationManager` is now `getFactory`
- `AnimationTestEvent` is now `AnimationEvent`
- `AnimationPredicate`'s have a return type of `PlayState` instead of a boolean
- You should no longer register your controllers in your animatable's constructor. Instead, do it in `registerControllers()`
- You can no longer store a reference to any `AnimationController`'s.
- Make sure you use one of the available `GeoRenderer's`, like `GeoEntityRenderer` or `GeoItemRenderer`