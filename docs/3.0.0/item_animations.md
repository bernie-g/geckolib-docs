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

### Syncing Item Animations

To register the animations to sync to other players in servers, simply implement [`ISyncable`](https://github.com/bernie-g/geckolib/blob/1.16/src/main/java/software/bernie/geckolib3/network/ISyncable.java). Then, override the necessary methods like below 


=== "Forge"
    ```java
	public class PistolItem extends Item implements IAnimatable, ISyncable {

		public AnimationFactory factory = new AnimationFactory(this);
		public String controllerName = "controller";
		public static final int ANIM_OPEN = 0;

		public PistolItem() {
			super(new Item.Properties().tab(GeckoLibMod.geckolibItemGroup).stacksTo(1).durability(201)
					.setISTER(() -> PistolRender::new));
			// Registers the item to sync
			GeckoLibNetwork.registerSyncable(this);
		}

		public <P extends Item & IAnimatable> PlayState predicate(AnimationEvent<P> event) {
			// this is set below
			return PlayState.CONTINUE;
		}

		@SuppressWarnings({ "unchecked", "rawtypes" })
		@Override
		public void registerControllers(AnimationData data) {
			data.addAnimationController(new AnimationController(this, controllerName, 1, this::predicate));
		}

		@Override
		public AnimationFactory getFactory() {
			return this.factory;
		}

		//This is where will be set the animation  we want to play for our item.
		@Override
		public void onAnimationSync(int id, int state) {
			if (state == ANIM_OPEN) {
				final AnimationController<?> controller = GeckoLibUtil.getControllerForID(this.factory, id, controllerName);
				if (controller.getAnimationState() == AnimationState.Stopped) {
					controller.markNeedsReload();
					controller.setAnimation(new AnimationBuilder().addAnimation("firing", false));
				}
			}
		}
		
		@Override
		public void releaseUsing(ItemStack stack, World worldIn, LivingEntity entityLiving, int timeLeft) {
			if (entityLiving instanceof PlayerEntity) {
				PlayerEntity playerentity = (PlayerEntity) entityLiving;
				if (stack.getDamageValue() < (stack.getMaxDamage() - 1)) {
					playerentity.getCooldowns().addCooldown(this, 5);
					if (!worldIn.isClientSide) {
						ArrowEntity abstractarrowentity = createArrow(worldIn, stack, playerentity);
						abstractarrowentity = customeArrow(abstractarrowentity);
						abstractarrowentity.shootFromRotation(playerentity, playerentity.xRot, playerentity.yRot, 0.0F,
								1.0F * 3.0F, 1.0F);

						abstractarrowentity.setBaseDamage(2.5);
						abstractarrowentity.tickCount = 35;
						abstractarrowentity.isNoGravity();

						stack.hurtAndBreak(1, entityLiving, p -> p.broadcastBreakEvent(entityLiving.getUsedItemHand()));
						worldIn.addFreshEntity(abstractarrowentity);
					}
					//This will send the animation to the other players.
					if (!worldIn.isClientSide) {	
						// Gets the item that the player is holding, should be this item.
						final int id = GeckoLibUtil.guaranteeIDForStack(stack, (ServerWorld) worldIn);
						// Tell all nearby clients to trigger this item to animate
						final PacketDistributor.PacketTarget target = PacketDistributor.TRACKING_ENTITY_AND_SELF
								.with(() -> playerentity);
						GeckoLibNetwork.syncAnimation(target, this, id, ANIM_OPEN);
					}
				}
			}
		}
	}
    ```
=== "Fabric"
    ```java
	public class PistolItem extends Item implements IAnimatable, ISyncable {

		public AnimationFactory factory = new AnimationFactory(this);
		public String controllerName = "controller";
		public static final int ANIM_OPEN = 0;

		public PistolItem() {
			super(new Item.Settings().group(ItemRegistry.geckolibItemGroup).maxCount(1).maxDamage(201));
			// Registers the item to sync
			GeckoLibNetwork.registerSyncable(this);
		}

		public <P extends Item & IAnimatable> PlayState predicate(AnimationEvent<P> event) {
			// this is set below
			return PlayState.CONTINUE;
		}

		@SuppressWarnings({ "unchecked", "rawtypes" })
		@Override
		public void registerControllers(AnimationData data) {
			data.addAnimationController(new AnimationController(this, controllerName, 1, this::predicate));
		}

		@Override
		public AnimationFactory getFactory() {
			return this.factory;
		}

		//This is where will be set the animation  we want to play for our item.
		@Override
		public void onAnimationSync(int id, int state) {
			if (state == ANIM_OPEN) {
				final AnimationController<?> controller = GeckoLibUtil.getControllerForID(this.factory, id, controllerName);
				if (controller.getAnimationState() == AnimationState.Stopped) {
					controller.markNeedsReload();
					controller.setAnimation(new AnimationBuilder().addAnimation("firing", false));
				}
			}
		}
		
		@Override
		public void onStoppedUsing(ItemStack stack, World worldIn, LivingEntity entityLiving, int remainingUseTicks) {
			if (entityLiving instanceof PlayerEntity) {
				PlayerEntity playerentity = (PlayerEntity) entityLiving;
				if (stack.getDamage() < (stack.getMaxDamage() - 1)) {
					playerentity.getItemCooldownManager().set(this, 5);
					if (!worldIn.isClient) {
						ArrowEntity abstractarrowentity = createArrow(worldIn, stack, playerentity);
						abstractarrowentity.setProperties(playerentity, playerentity.pitch, playerentity.yaw, 0.0F,
								1.0F * 3.0F, 1.0F);

						abstractarrowentity.setDamage(2.5);
						abstractarrowentity.age = 35;
						abstractarrowentity.hasNoGravity();

						stack.damage(1, entityLiving, p -> p.sendToolBreakStatus(entityLiving.getActiveHand()));
						worldIn.spawnEntity(abstractarrowentity);
					}
					if (!worldIn.isClient) {
						// Gets the item that the player is holding, should be this item.
						final int id = GeckoLibUtil.guaranteeIDForStack(stack, (ServerWorld) worldIn);
						GeckoLibNetwork.syncAnimation(playerentity, this, id, ANIM_OPEN);
						// Tell all nearby clients to trigger this item to animate
						for (PlayerEntity otherPlayer : PlayerLookup.tracking(playerentity)) {
							GeckoLibNetwork.syncAnimation(otherPlayer, this, id, ANIM_OPEN);
						}
					}
				}
			}
		}
	}
    ```

