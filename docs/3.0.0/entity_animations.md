!!! note
    This wiki page assumes you are in the `Entity` setting in your GeckoLib blockbench model. Learn how to change this [here](../getting_started/#creating-a-model)
# Entity Animations
In order to create an entity with GeckoLib you need to do several things.

1. Create the Entity class (extends `LivingEntity` or any subtype)
2. Create the Renderer class (extends `GeoEntityRenderer`)
3. Create the [Model](models.md) class (extends `AnimatedGeoModel`)

??? fabric
    This wiki almost exclusively uses MCP mappings. If you need to figure out the yarn name for a MCP class/field/method, check out the [`fabric`](https://github.com/bernie-g/geckolib/tree/fabric/1.16) branch of geckolib or use the linkie bot in the GeckoLib [discord](https://discord.com/invite/MNQcKxB).
    
## Entity Class
Any GeckoLib object has to extend [`IAnimatable`](https://github.com/bernie-g/geckolib-core/blob/master/src/main/java/software/bernie/geckolib/core/IAnimatable.java). This exposes two methods you need to override:

- `getFactory()` - All you need to here is return a member field of type `AnimationFactory`. This needs to be passed in an instance of your entity (usually done by passing in `this`)
- `registerControllers` - This method is called everytime a new instance of your animatable object (in this case your entity) is created. So, you should create your animation controllers, register listeners, and do any initial setup here. Take a look at the example below for a more obvious example. Although not recommended, you can store a reference to any animation controllers created in this method **only for entities**.

### Entity Class Example
=== "Forge"
    ```java
    public class GeoExampleEntity extends CreatureEntity implements IAnimatable
    {
        private AnimationFactory factory = new AnimationFactory(this);
    
        public GeoExampleEntity(EntityType<? extends CreatureEntity> type, World worldIn)
        {
            super(type, worldIn);
            this.ignoreFrustumCheck = true;
        }
        
        private <E extends IAnimatable> PlayState predicate(AnimationEvent<E> event)
        {
            event.getController().setAnimation(new AnimationBuilder().addAnimation("animation.bat.fly", true));
            return PlayState.CONTINUE;
        }
    
        @Override
        public void registerControllers(AnimationData data)
        {
            data.addAnimationController(new AnimationController(this, "controller", 0, this::predicate));
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
    public class GeoExampleEntity extends PathAwareEntity implements IAnimatable
    {
        private AnimationFactory factory = new AnimationFactory(this);
    
        public GeoExampleEntity(EntityType<? extends PathAwareEntity> type, World worldIn)
        {
            super(type, worldIn);
            this.ignoreCameraFrustum = true;
        }
    
        private <E extends IAnimatable> PlayState predicate(AnimationEvent<E> event)
        {
            event.getController().setAnimation(new AnimationBuilder().addAnimation("animation.bat.fly", true));
            return PlayState.CONTINUE;
        }
    
        @Override
        public void registerControllers(AnimationData data)
        {
            data.addAnimationController(new AnimationController(this, "controller", 0, this::predicate));
        }
    
        @Override
        public AnimationFactory getFactory()
        {
            return this.factory;
        }
    }
    ```
    
### Registering the Entity
To register the entity, either use [Registry Events](https://mcforge.readthedocs.io/en/latest/concepts/registries/#deferredregister) or preferably [Deferred Registries](https://mcforge.readthedocs.io/en/latest/concepts/registries/#deferredregister).


## Renderer
Entity renderers are fairly simple. All you need to do is create a renderer class that extends `GeoEntityRenderer`, like this:

=== "Forge"
    ```java
    public class ExampleGeoRenderer extends GeoEntityRenderer<GeoExampleEntity>
    {
        public ExampleGeoRenderer(EntityRendererManager renderManager)
        {
            super(renderManager, new ExampleEntityModel());
			this.shadowSize = 0.7F; //change 0.7 to the desired shadow size.
        }
    }
    ```
=== "Fabric"
    ```java
    public class ExampleGeoRenderer extends GeoEntityRenderer<GeoExampleEntity>
    {
    	public ExampleGeoRenderer(EntityRenderDispatcher renderManager)
    	{
    		super(renderManager, new ExampleEntityModel());
			this.shadowRadius = 0.7F; //change 0.7 to the desired shadow size.
    	}
    }
    ```
Make sure to pass in the correct [model](models.md) class into the super constructor.

### Registering the Renderer
If you don't register the renderer, your game will crash (in 1.15+) or show as a white box (in 1.12). 
To register your renderer, place this method call in your `FMLClientSetupEvent` event handler (or `onInitializeClient` in Fabric`:
=== "Forge"
    ```java
    @OnlyIn(Dist.CLIENT)
    @SubscribeEvent
    public static void registerRenderers(final FMLClientSetupEvent event)
    {
        RenderingRegistry.registerEntityRenderingHandler(EntityRegistry.GEO_EXAMPLE_ENTITY.get(),
        manager -> new ExampleGeoRenderer(manager));
    }
    ```
=== "Fabric"
    ```java
    @Override
    public void onInitializeClient() 
    {
        EntityRendererRegistry.INSTANCE.register(EntityRegistry.GEO_EXAMPLE_ENTITY, 
        (entityRenderDispatcher, context) -> new ExampleGeoRenderer(entityRenderDispatcher));
    }
    ```

## Model
Entity models in GeckoLib are the same as models for any other animation type. See the article on [models](models.md).
