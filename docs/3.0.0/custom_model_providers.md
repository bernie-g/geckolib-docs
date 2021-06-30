# Abstract
Part of the big update in GeckoLib 3.0.0 was the decoupling between animation code and rendering code. All animation code was refactored into [geckolib-core](https://github.com/bernie-g/geckolib-core). This allows us to animate much more than just entities. Moreover, end-users like you can now add your own models and apply geckolib to almost anything. For example, you could animate guis, player animations, horse armor, or pretty much anything else. This section is pretty advanced, and you will likely need help if you decide to implement your own model type. Feel free to ask questions in our discord.

## Creating your own model type
Geckolib fundamentally requires 3 parts. A model, which you probably won't need to do anything with. A renderer, which you'll need to make. An animatable object, which implements `IAnimatable`. 

### Custom Renderer
Your renderer class should implement `IGeoRenderer`. You'll need to implement all the required methods, such as `getGeoModelProvider` and `getTextureLocation`. Additionally, if necessary you should implement `getUniqueID` if your animatable is a singleton like an Item.

Then, to actually render you will need an entry point. For entities, geckolib's simply registers to RenderingRegistry and then Forge/Fabric calls the render method. For tiles, geckolib simply hooks into the tile entity render method. If you don't have access to any of these, you might need to use a forge/fabric render event or mixin into somewhere where you have an `IRenderTypeBuffer` or `IVertexBuilder` available. Then, simply call `IGeoRenderer.render()` with all the required parameters. Please note that IGeoRenderer takes in both an IRenderTypeBuffer and an IVertexBuilder. However, either one of these can be null and it will still work fine. This is for scenarios like armor renderers where you only have access to an IVertexBuilder.

After you've created your renderer, you'll probably need to create a renderer registry, similar to the `GeoArmorRenderer` registry. Just create a static map and a few registry helper methods. This will be used when you register your model fetcher.

### Model Fetchers
When GeckoLib animates, it only has access to an `IAnimatable`. This means it needs to somehow find the model associated for a particular animatable instance. This is done by querying all registered model fetchers and using the first one that returns a model. In essence, a model fetcher is simply a function that takes an `IAnimatable` and returns a `IAnimatableModel`, such as an `AnimatedGeoModel`. There are currently 5 registered model fetchers: tile, item, entity, armor, and replaced entity. If you create your own model type, you'll need to register a model fetcher for that type. This can be done in a static block, such as in `GeoArmorRenderer`. Simply call `#!java AnimationController.addModelFetcher()` with a lambda expression or method reference.
