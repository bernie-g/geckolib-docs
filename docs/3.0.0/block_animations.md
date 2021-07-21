!!! note
    This wiki page assumes you are in the `Block/Item` setting in your GeckoLib blockbench model. Learn how to change this [here](../getting_started/#creating-a-model)
    
# Block Animations
Blocks can only be animated with GeckoLib if they have a `TileEntity` (or `BlockEntity` in Yarn).

## Creating a TileEntity
Read this [forge article](https://mcforge.readthedocs.io/en/latest/tileentities/tileentity/) or this [fabric article](https://fabricmc.net/wiki/tutorial:blockentity) to learn how to create a block with an attached `TileEntity`.

!!! note
    From now on, I will refer to them only as TileEntities. Fuck you Yarn, I don't care that it's a logically better name in every single way.

After you do that, make your TileEntity implement [`IAnimatable`](https://github.com/bernie-g/geckolib-core/blob/master/src/main/java/software/bernie/geckolib/core/IAnimatable.java). Then, override the same methods as you would for an animated entity.

### Disabling the Vanilla Renderer
By default, Minecraft will search for a blockstate file in your assets folder to figure out how to render your block. Since the GeckoLib renderer does everything differently, you'll need to disable this functionality.

All you need to do is override this method in your block class:

```java
@Override
public BlockRenderType getRenderType(BlockState state)
{
	return BlockRenderType.ENTITYBLOCK_ANIMATED;
}
```

??? fabric
    Somehow, this method has exactly the same mappings in Yarn as it does in MCP. 
    `¯\_(ツ)_/¯`
    
## Creating a Tile Entity Renderer
To create the GeckoLib tile entity renderer, simply create a new class that extends `GeoBlockRenderer`. Then, pass in a new instance of your model in the super constructor call.

=== "Forge"
    ```java
    public class FertilizerTileRenderer extends GeoBlockRenderer<FertilizerTileEntity>
    {
        public FertilizerTileRenderer(TileEntityRendererDispatcher rendererDispatcherIn)
        {
            super(rendererDispatcherIn, new FertilizerModel());
        }
    }
    ```
=== "Fabric"
    ```java
    public class FertilizerTileRenderer extends GeoBlockRenderer<FertilizerTileEntity>
    {
    	public FertilizerTileRenderer(BlockEntityRenderDispatcher rendererDispatcherIn)
    	{
    		super(rendererDispatcherIn, new FertilizerModel());
    	}
    }
    ```
    
    
### Registering the Renderer
To register your renderer, place this method call in your `FMLClientSetupEvent` event handler:

=== "Forge"
    ```java
    @OnlyIn(Dist.CLIENT)
    @SubscribeEvent
    public static void registerRenderers(final FMLClientSetupEvent event)
    {
        ClientRegistry.bindTileEntityRenderer(TileRegistry.FERTILIZER.get(), FertilizerTileRenderer::new);
    }
    ```
=== "Fabric"
    ```java
    @Override
    public void onInitializeClient() 
    {
        BlockEntityRendererRegistry.INSTANCE.register(TileRegistry.FERTILIZER, FertilizerTileRenderer::new);
    }
    ```
    
???+ note
    GeckoLib handles block rotations automatically, so if your block is directional the GeckoLib renderer will turn it automatically. However, if you need to do something special with rotations or disable them completely, simply override `rotateBlock` in `GeoBlockRenderer`
???+ bug
    To make your block transparent or translucent, set the render layer in your Renderer by overriding `getRenderType` in your renderer class. This  applies to entities, items, and armor too but it's most commonly used in blocks.
    For some reason, `RenderType.getEntityTranslucent` seems to not work when facing water or some other entities. We're not sure why; it seems to be a limitation of Minecraft's rendering engine. If you have any clues as to why this is so, please let us know.
