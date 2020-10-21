!!! note
    This wiki page assumes you are in the `Block/Item` setting in your GeckoLib blockbench model. Learn how to change this [here](../getting_started/#creating-a-model)

# Item Animations
Item animations in GeckoLib are a bit tricky. Unlike Entities and TileEntities, Items are effectively singletons. The non-singleton wrapper object for Items is `ItemStack`s, but unfortunately we don't have much control over this. Therefore, pay close attention to how GeckoLib handles item differentiation, unless you want all your items animating at the same time.

## Creating An Animated Item
The process of creating an animated item is straightforward enough. Just make your item class implement [`IAnimatable`](https://github.com/bernie-g/geckolib-core/blob/master/src/main/java/software/bernie/geckolib/core/IAnimatable.java). Then, override the necessary methods [like usual](../entity_animations/#entity-class).

An example animated item class:

=== "Forge"
    ```java
    public class JackInTheBoxItem extends Item implements IAnimatable
    {
    	public AnimationFactory factory = new AnimationFactory(this);
    
    	public JackInTheBoxItem(Properties properties)
    	{
    		super(properties);
    	}
    	
    	private <P extends Item & IAnimatable> PlayState predicate(AnimationEvent<P> event) 
    	{
            event.getController().setAnimation(new AnimationBuilder().addAnimation("Soaryn_chest_popup", true));
            return PlayState.CONTINUE;
        }
    
    	@Override
    	public void registerControllers(AnimationData data)
    	{
            data.addAnimationController(new AnimationController(this, "controller", 20, this::predicate));
    	}
    
    	@Override
    	public AnimationFactory getFactory()
    	{
    		return this.factory;
    	}
    }
    ```
=== "Fabric"
    ```java
    public class JackInTheBoxItem extends Item implements IAnimatable
    {
        public AnimationFactory factory = new AnimationFactory(this);
    
        public JackInTheBoxItem(Settings properties)
        {
            super(properties);
        }
    
        private <P extends Item & IAnimatable> PlayState predicate(AnimationEvent<P> event)
        {
            event.getController().setAnimation(new AnimationBuilder().addAnimation("Soaryn_chest_popup", true));
            return PlayState.CONTINUE;
        }
    
        @Override
        public void registerControllers(AnimationData data)
        {
            data.addAnimationController(new AnimationController(this, "controller", 20, this::predicate));
        }
    
        @Override
        public AnimationFactory getFactory()
        {
            return this.factory;
        }
    }
    ```

## Creating an Item Renderer
Simply make a class that extends `GeoItemRenderer`, like so:

=== "Forge"
    ```java
    public class JackInTheBoxRenderer extends GeoItemRenderer<JackInTheBoxItem>
    {
    	public JackInTheBoxRenderer()
    	{
    		super(new JackInTheBoxModel());
    	}
    }
    ```
=== "Fabric"
    ```java
    public class JackInTheBoxRenderer extends GeoItemRenderer<JackInTheBoxItem>
    {
    	public JackInTheBoxRenderer()
    	{
    		super(new JackInTheBoxModel());
    	}
    }
    ```
    
### Registering the Item Renderer

To register the renderer, simply call the `setISTER()` method on your `Item.Properties` object.

=== "Forge"
    ```java
    new JackInTheBoxItem(new Item.Properties().setISTER(() -> JackInTheBoxRenderer::new))
    ```
???+ fabric
    In fabric, you have to register the Renderer in your `onInitializeClient` method, like so:
    ```java
    @Override
    public void onInitializeClient() 
    {
        GeoItemRenderer.registerItemRenderer(ItemRegistry.JACK_IN_THE_BOX, new JackInTheBoxRenderer());
    }
    ```
    
## Item Display Settings

!!! warning
    If you skip this step, your item will not render at all. Moreover, make sure to not remove the parent property in your exported display settings, or else the model will not render either.
First, export your item's display settings from blockbench as described [here](../getting_started/#exporting-your-model)
Then, place the exported `.json` file in `resources/assets/<modid>/models/item`. Make sure the json file has the same name as your item's registry name.

## The Singleton Issue
Since Items are essentially singletons, we have to do some extra work to make them animated properly. 

- Make sure you never store a reference to any `AnimationController`'s you create. Those should only be created in `#!java registerAnimationControllers()` and added to the `AnimationData` in that method.
- When you have access to an `AnimationEvent` or a `KeyframeEvent`, you can get access to that itemstack's controller using `#!java getController()`
- If you do not have access to either of those objects, such as when you're in `#!java onItemRightClick()`, you can get the right `AnimationController` using `#!java GeckoLibUtil.getControllerForStack(this.factory, stack, controllerName);`

If you are having trouble with this concept, look at the JackInTheBoxItem example in GeckoLib's example mod. It's thoroughly commented and should help clear up the concept.

### Changing Uniqueness Criteria
Currently, the only way GeckoLib can identify that two ItemStacks are different and should be animated independently is based on their item class, count, and ItemStack nbt tag. This means that if you have two ItemStacks in your inventory each with the same amount of your item and same NBT tag, they will be animated simultaneously. The best solution to this is to assign each ItemStack a unique ID and store it in NBT. Then, they will be animated independently.

If you need to control the criteria that GeckoLib uses to determine that two ItemStacks are different, override `#!java getUniqueID` in your `GeoItemRenderer`. You can also override this method for entities, tile entities, and armor, although it's most useful for items and armor. Most people will never need to do this.


