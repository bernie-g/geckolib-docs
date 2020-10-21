!!! note
    This wiki page assumes you are in the `Armor` setting in your GeckoLib blockbench model. Learn how to change this [here](../getting_started/#creating-a-model). This model type is special in the fact that it generates an armor template for you automatically. Please do not rename any of the default included bones unless you really need to.
    
# Armor Animations
To create animated armor you must first create an Item that extends `GeoArmorItem` and implements [`IAnimatable`](https://github.com/bernie-g/geckolib-core/blob/master/src/main/java/software/bernie/geckolib/core/IAnimatable.java)

Similarly to Items, make sure to follow the [guidelines when animating singleton objects](../item_animations/#the-singleton-issue)

=== "Forge"
    ```java
    public class PotatoArmorItem extends GeoArmorItem implements IAnimatable
    {
    	private AnimationFactory factory = new AnimationFactory(this);
    
    	public PotatoArmorItem(IArmorMaterial materialIn, EquipmentSlotType slot, Properties builder)
    	{
    		super(materialIn, slot, builder.group(ItemGroup.COMBAT));
    	}
    
    	private <P extends IAnimatable> PlayState predicate(AnimationEvent<P> event)
    	{
            event.getController().setAnimation(new AnimationBuilder().addAnimation("animation.potato_armor.new", true));
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
    public class PotatoArmorItem extends GeoArmorItem implements IAnimatable 
    {
        private AnimationFactory factory = new AnimationFactory(this);
    
        public PotatoArmorItem(ArmorMaterial materialIn, EquipmentSlot slot, Item.Settings builder) 
        {
            super(materialIn, slot, builder.group(ItemGroup.COMBAT));
        }
    
    	private <P extends IAnimatable> PlayState predicate(AnimationEvent<P> event)
    	{
            event.getController().setAnimation(new AnimationBuilder().addAnimation("animation.potato_armor.new", true));
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
    
    
## Creating an Armor Renderer
Out of all the GeoRenderers, `GeoArmorRenderer` requires the most work. After you create your renderer, you need to make sure ever armor bone matches up to the GeckoLib armor bone names. By default, the bones have the same names as the blockbench template, but if you want to modify them, set their names in the constructor.

=== "Forge"
    ```java
    public class PotatoArmorRenderer extends GeoArmorRenderer<PotatoArmorItem>
    {
    	public PotatoArmorRenderer()
    	{
    		super(new PotatoArmorModel());
    
    		//These values are what each bone name is in blockbench. So if your head bone is named "bone545", make sure to do this.headBone = "bone545";
    		// The default values are the ones that come with the default armor template in the geckolib blockbench plugin.
    		this.headBone = "helmet";
    		this.bodyBone = "chestplate";
    		this.rightArmBone = "rightArm";
    		this.leftArmBone = "leftArm";
    		this.rightLegBone = "rightLeg";
    		this.leftLegBone = "leftLeg";
    		this.rightBootBone = "rightBoot";
    		this.leftBootBone = "leftBoot";
    	}
    }
    ```

=== "Fabric"
    ```java
    public class PotatoArmorRenderer extends GeoArmorRenderer<PotatoArmorItem> 
    {
        public PotatoArmorRenderer() 
        {
            super(new PotatoArmorModel());
    
            //These values are what each bone name is in blockbench. So if your head bone is named "bone545", make sure to do this.headBone = "bone545";
            // The default values are the ones that come with the default armor template in the geckolib blockbench plugin.
            this.headBone = "helmet";
            this.bodyBone = "chestplate";
            this.rightArmBone = "rightArm";
            this.leftArmBone = "leftArm";
            this.rightLegBone = "rightLeg";
            this.leftLegBone = "leftLeg";
            this.rightBootBone = "rightBoot";
            this.leftBootBone = "leftBoot";
        }
    }
    ```
### Registering the Armor Renderer

=== "Forge"
    ```java
    @OnlyIn(Dist.CLIENT)
    @SubscribeEvent
    public static void registerRenderers(final FMLClientSetupEvent event)
    {
        GeoArmorRenderer.registerArmorRenderer(PotatoArmorItem.class, new PotatoArmorRenderer());
    }
    ```
=== "Fabric"
    ```java
    @Override
    public void onInitializeClient()
    {
        GeoArmorRenderer.registerArmorRenderer(PotatoArmorItem.class, new PotatoArmorRenderer());
    }
    ```