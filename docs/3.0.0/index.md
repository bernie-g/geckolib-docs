!!! warning
    GeckoLib 3.0.0 is currently in alpha. **Do not use this version of geckolib in production, a released mod, or anywhere that can't handle unexpected crashes**. Moreover, this version is only available on Forge 1.15.2, and there is no jar released on the curseforge page.

 
# Installation
## GeckoLib Library
To install the actual geckolib forge library, insert this dependency snippet into your build.gradle. Do not put this in your buildscript section.
```groovy
repositories{
    maven { url 'https://repo.repsy.io/mvn/gandiber/geckolib' }
}
dependencies{
    compile fg.deobf('software.bernie.geckolib:forge-1.15.2-geckolib:3.0.0-alpha.1')
}
```

## Blockbench Plugin
To install the alpha blockbench plugin use the githack URL provided to you 
[https://rawcdn.githack.com/fadookie/geckolib-plugin/30e971d829202882fa11eb3c1e903eefcd3a286a/plugins/animation_utils.js](https://rawcdn.githack.com/fadookie/geckolib-plugin/30e971d829202882fa11eb3c1e903eefcd3a286a/plugins/animation_utils.js) 

1. Close any GeckoLib project.
2. Open Filter -> Plugins Menu: 
3. If you have GeckoLib Animation Utils plugin installed already, uninstall it:
<img width="657" alt="Screen Shot 2020-10-10 at 11 08 25 AM" src="https://user-images.githubusercontent.com/110764/95662060-0441a200-0ae9-11eb-86c0-68df8943b40e.png">
4. Click "Load Plugin from URL".
![image](https://user-images.githubusercontent.com/110764/95662058-0146b180-0ae9-11eb-93c5-3a6887a05c2e.png)
5. Paste in the provided githack.com URL.
<img width="555" alt="Screen Shot 2020-10-10 at 11 09 14 AM" src="https://user-images.githubusercontent.com/110764/95662062-0d327380-0ae9-11eb-8101-01f942017267.png">

