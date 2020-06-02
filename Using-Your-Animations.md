# Adding Animated Entities Into Your Mod

# Base Setup
Adding an entity with GeckoLib is very similar to how you would normally do it in a forge mod. You need to create a model, renderer, and entity class, register the entity using either a deferred registry or the normal registry events, and register the renderer using `RenderingRegistry.registerEntityRenderingHandler`, or your forge version's equivalent.

# Model

GeckoLib requires some extra stuff in the model class than the usual things you would do in forge. First, you add the exported .java model into your mod, and add all the required imports/replace the entity names with your own entity. The GeckoLib blockbench plugin also generates the `getAnimationFileLocation()` method, where you should replace the ResourceLocation with a path to your json animation file (exported from blockbench).

# Renderer
Every entity needs it's own renderer, and GeckoLib makes no changes to how EntityRenderer's work. Here is an example EntityRenderer from GeckoLib's test entities:

```java
@OnlyIn(Dist.CLIENT)
public class TigrisRenderer extends MobRenderer<TigrisEntity, TigrisModel>
{
	public TigrisRenderer(EntityRendererManager rendererManager)
	{
		super(rendererManager, new TigrisModel(), 0.5F);
	}

	@Nullable
	@Override
	public ResourceLocation getEntityTexture(TigrisEntity entity)
	{
		return new ResourceLocation("geckolib" + ":textures/model/entity/tigris.png");
	}

	@Override
	protected void applyRotations(TigrisEntity entityLiving, MatrixStack matrixStackIn, float ageInTicks, float rotationYaw, float partialTicks)
	{
		super.applyRotations(entityLiving, matrixStackIn, ageInTicks, rotationYaw, partialTicks);
	}
}
```

## Entity
The entity class is where most of the animation controlling code will be located. There are several things you need to do to the entity class in order to get up and running with GeckoLib:

1. Implement `IAnimatedEntity`
2. Create a field of type `AnimationControllerCollection`. To learn more about animation controller's see [the wiki page](https://github.com/bernie-g/geckolib/wiki/Animation-Controllers)
3. Create as many `AnimationController`'s as you need, declaring each as a field.
4. Override `getAnimationControllers()`, and return your `AnimationControllerCollection`.
5. Create a method `registerAnimationControllers()` where you add all your `AnimationController`s to your `AnimationControllerCollection`, and **call this method in the entity constructor.** If you don't call this method in the constructor, none of your animations will play.

### Animation Predicates
Every render frame, GeckoLib will process every `AnimationController` and execute it's associated `AnimationPredicate`. An Animation Predicate is where you should set your animations, stop them, and transition between them. Feel free to run the `AnimationController#setAnimation()` method every tick, it won't restart the animation unless you change the Animation being run. Additionally, if you want to stop the animation from executing, you'll need to `return false`. If you want the animation to continue executing, `return true`.

### Example Entity Class
```java
public class TigrisEntity extends GhastEntity implements IAnimatedEntity
{
	public AnimationControllerCollection animationControllers = new AnimationControllerCollection();
	private AnimationController moveController = new AnimationController(this, "moveController", 10F, this::moveController);

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

	public TigrisEntity(EntityType<? extends GhastEntity> type, World worldIn)
	{
		super(type, worldIn);
		registerAnimationControllers();
	}

	@Override
	public AnimationControllerCollection getAnimationControllers()
	{
		return animationControllers;
	}

	public void registerAnimationControllers()
	{
		if(world.isRemote)
		{
			this.animationControllers.addAnimationController(moveController);
		}
	}
}
```