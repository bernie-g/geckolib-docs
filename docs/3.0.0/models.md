# Models
In Geckolib, models are the glue that put together your models, animations, and textures.
A standard model class is very simple and can be reused.

To create a model class extend `AnimatedGeoModel` and implement all the required methods.

The class should look like this:
```java
public class JackInTheBoxModel extends AnimatedGeoModel<JackInTheBoxItem>
{
	@Override
	public ResourceLocation getModelLocation(JackInTheBoxItem object)
	{
		return new ResourceLocation(GeckoLib.ModID, "geo/jack.geo.json");
	}

	@Override
	public ResourceLocation getTextureLocation(JackInTheBoxItem object)
	{
		return new ResourceLocation(GeckoLib.ModID, "textures/item/jack.png");
	}

	@Override
	public ResourceLocation getAnimationFileLocation(JackInTheBoxItem object)
	{
		return new ResourceLocation(GeckoLib.ModID, "animations/jackinthebox.animation.json");
	}
}
```

Each of these methods handles the location of the model, texture, and animation file respectively.
!!! note
    If you want to reuse a model class for say both an entity animation and an item animation, simply remove the generic parameter of AnimatedGeoModel and use the raw type.
!!! fabric
    In fabric (yarn), a `ResourceLocation` is called an `Identifier`
## Resource Locations
Your resources cannot go in any directory, otherwise geckolib will not be able to find them during the resource scanning phase. Make sure all your resources are in the correct folder. The reason this is strict is so that geckolib can load your models at runtime and make it easier for people overriding your models/animations in resource packs.

### Models (.geo.json)
Model files have to go in `resources/assets/<modid>/geo`. They can also be in any subdirectory of the geo folder, so feel free to organize your models however you wish. 

### Animation Files (.animation.json)
Animation files have to go in `resources/assets/<modid>/animations`. Similarly to model files, animations can go in any subfolder of `/animations`

### Textures (.png)
Textures are the same as in vanilla. They must go in `resources/assets/<modid>/textures`, or any subdirectory of that folder.

