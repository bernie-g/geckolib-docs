# Examples
GeckoLib comes packaged with an examplemod. It is not built in the final jar, but you can run it and mess with it by cloning the geckolib repo and running it like a normal mod.

## Entities
GeckoLib has two example entities, a [bat](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/example/entity/GeoExampleEntity.java) that uses molang for all it's animations, and a [bike](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/example/entity/BikeEntity.java) that showcases some more advanced molang queries. This section will be updated with more complex entities soon.


## Blocks
GeckoLib has the [botarium](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/example/block/tile/BotariumTileEntity.java) and [fertilizer](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/example/block/tile/FertilizerTileEntity.java) example block models. The botarium simply opens up over and over again and the fertilizer switches models, textures, and animations based on the current rain state. Again, this section will be updated soon.


## Items
The [`JackInTheBoxItem`](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/example/item/JackInTheBoxItem.java) is the most detailed and well-commented example so far. Make sure you take a look at it, it serves as a good example. The item plays music and an animation on right click.


## Armor
The [`PotatoArmorItem`](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/example/item/PotatoArmorItem.java) is the second most detailed example. It plays the armor animation only when all 4 armor pieces are put on.


## Replaced Entities
The only replaced entity example is the [`ReplacedCreeperEntity`](https://github.com/bernie-g/geckolib/blob/develop/src/main/java/software/bernie/example/entity/ReplacedCreeperEntity.java), which is self-explanatory.