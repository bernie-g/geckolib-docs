# The Bedrock Renderer
GeckoLib's rendering code is mostly in `IGeoRenderer`. Since your renderer class implements that interface, you can override many of the methods to change rendering properties.

## Changing Colors
Overriding `getRenderColor()` lets you change the tint and alpha that your model renders in. Simply return a `Color` value.

## Changing Render Types
Override `getRenderType()` to change the `RenderType` that your model uses. You **must** use one of the entity types, even for blocks and items. This is because GeckoLib does not bake textures into the texture atlas, and instead renders from individual files. The default `RenderType` is cutout, but you can change it to translucent or solid if you wish.

## Custom Rendering
Sometimes you want to render extra things with your model, such as an item or block. You can do this in `renderEarly`, which is before the actual model has been rendered, or `renderLate`, which is after.