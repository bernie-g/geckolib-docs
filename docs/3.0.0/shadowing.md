!!! info
    Shadowing should only be done if you really need it. Not only does it prevent GeckoLib from getting downloads on Forge for your mod (and subsequently prevents income from your mod), it also can be tricky to setup. Please only do this if you have a legitimate reason. If you do decide to shadow, consider donating to our [patreon!](https://www.patreon.com/geckosmods) 
    
# Shadowing

In order to shadow GeckoLib, simply follow the [shadow's plugin documentation](https://imperceptiblethoughts.com/shadow/introduction/). Make sure to relocate geckolib to it's own package.

You can view an example `build.gradle` that shadows geckolib [here](https://github.com/Heroes-United/HeroesUnited/blob/faa76bd184959f4616037b38ceda1201f66a919c/build.gradle#L85-L87)

You will also need to do a few extra steps if shadowing with Forge, instead of [Initializing GeckoLib](https://geckolib.com/en/latest/3.0.0/) like normally, you'll want to use the code below:
```java
    GeckoLib.hasInitialized = true;
    DistExecutor.safeRunWhenOn(Dist.CLIENT, () -> ResourceListener::registerReloadListener);
```

This is needed as Geckolib now ships with a needed Packet for Item syncing on servers. If you wish to use this feature when shadowing GeckoLib, you will need to create your own Packet based off [GeckoLibNetwork](https://github.com/bernie-g/geckolib/blob/1.16/src/main/java/software/bernie/geckolib3/network/GeckoLibNetwork.java) and [SyncAnimationMsg](https://github.com/bernie-g/geckolib/blob/62137dd37927a3cfe6264499e9c0449afe8ed21f/src/main/java/software/bernie/geckolib3/network/messages/SyncAnimationMsg.java) replacing any calls to GeckoLibNetwork with your own Packet.

When you go to make your Item syncable via the ISyncable system, make sure to use your own Packet in place of these two calls to GeckoLibNetwork.
[Call 1](https://github.com/bernie-g/geckolib/blob/62137dd37927a3cfe6264499e9c0449afe8ed21f/src/main/java/software/bernie/example/item/PistolItem.java#L43)
[Call 2](https://github.com/bernie-g/geckolib/blob/62137dd37927a3cfe6264499e9c0449afe8ed21f/src/main/java/software/bernie/example/item/PistolItem.java#L69)
