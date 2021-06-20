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

## Rendering Items on Entities
Sometimes you want to have your entity to have an item, Geckolib now has predefined Itemstacks you can now call in your render code to grab an item. 

Predefined ItemStacks:

 - `mainHand` - Renders the item in the Entities Main Hand slot.
 - `offHand` - Renders the item in the Entities Off Hand slot.
 - `helmet` - Renders the item in the Entities Helmet Armor slot.
 - `chestplate` - Renders the item in the Entities Chestplate Armor slot.
 - `leggings` - Renders the item in the Entities Leggings Armor slot.
 - `boots` - Renders the item in the Entities Boots Armor slot.

To render the item with your entity, simply call `renderRecursively` in your entities render like so.

=== "Forge"
    ```java
	@Override
	public void renderRecursively(GeoBone bone, MatrixStack stack, IVertexBuilder bufferIn, int packedLightIn, int packedOverlayIn, float red, float green, float blue, float alpha) {
		if (bone.getName().equals("rArmRuff")) { // rArmRuff is the name of the bone you to set the item to attach too. Please see Note
			stack.push();
            //You'll need to play around with these to get item to render in the correct orientation
			stack.rotate(Vector3f.XP.rotationDegrees(-75)); 
			stack.rotate(Vector3f.YP.rotationDegrees(0)); 
			stack.rotate(Vector3f.ZP.rotationDegrees(0));
            //You'll need to play around with this to render the item in the correct spot.
			stack.translate(0.4D, 0.3D, 0.6D); 
            //Sets the scaling of the item.
			stack.scale(1.0f, 1.0f, 1.0f); 
            // Change mainHand to predefined Itemstack and TransformType to what transform you would want to use.
			Minecraft.getInstance().getItemRenderer().renderItem(mainHand, TransformType.THIRD_PERSON_RIGHT_HAND, packedLightIn, packedOverlayIn, stack, this.rtb);
			stack.pop();
			bufferIn = rtb.getBuffer(RenderType.entityTranslucent(whTexture));
		}
		super.renderRecursively(bone, stack, bufferIn, packedLightIn, packedOverlayIn, red, green, blue, alpha);
	}
    ```
=== "Fabric"
    ```java
	@Override
	public void renderRecursively(GeoBone bone, MatrixStack stack, VertexConsumer bufferIn, int packedLightIn, int packedOverlayIn, float red, float green, float blue, float alpha) {
		if (bone.getName().equals("rArmRuff")) { // rArmRuff is the name of the bone you to set the item to attach too. Please see Note
			stack.push();
            //You'll need to play around with these to get item to render in the correct orientation
			stack.multiply(Vector3f.POSITIVE_X.getDegreesQuaternion(-75)); 
			stack.multiply(Vector3f.POSITIVE_Y.getDegreesQuaternion(0)); 
			stack.multiply(Vector3f.POSITIVE_Z.getDegreesQuaternion(0)); 
            //You'll need to play around with this to render the item in the correct spot.
			stack.translate(0.4D, 0.3D, 0.6D); 
            //Sets the scaling of the item.
			stack.scale(1.0f, 1.0f, 1.0f); 
            // Change mainHand to predefined Itemstack and Mode to what transform you would want to use.
			MinecraftClient.getInstance().getItemRenderer().renderItem(mainHand, Mode.THIRD_PERSON_RIGHT_HAND, packedLightIn, packedOverlayIn, stack, this.rtb);
			stack.pop();
			bufferIn = rtb.getBuffer(RenderLayer.getEntityTranslucent(whTexture));
		}
		super.renderRecursively(bone, stack, bufferIn, packedLightIn, packedOverlayIn, red, green, blue, alpha);
	}
    ```
    
!!! note
    Note it is recommended to not use the parent bone to attach your item. If you do not have a child bone, it is suggested to simply make a small invisible child bone in your model to attach the item. 

## Resource Packs
All GeckoLib models and animations can be overriden in resource packs. This opens up a ton of possibilities, since before GeckoLib it was impossible to override models/animations for entities, much less animations. To create a resource pack that overrides geckolib models/animations, simply do everything how you would with a normal resource pack, and place your models in `/geo` and animations in `/animations`. That's it! Just press F3+T and your changes will instantly be reloaded in game.
