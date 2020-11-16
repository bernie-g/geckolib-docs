!!! warning
    GeckoLib 3.0.0 is currently in beta. It is currently only available on Forge 1.15.2, 1.16.4, and Fabric 1.16.4. We are in the process of backporting to 1.12.

 
# Installation
## Installing the Library
To install the actual geckolib forge library, insert this dependency snippet into your build.gradle. 

=== "Forge 1.15.2"
    ```groovy
    repositories{
        maven { url 'https://repo.repsy.io/mvn/gandiber/geckolib' }
    }
    dependencies{
        compile fg.deobf('{{ version_info_3.forge_1_15 }}')
    }
    ```
=== "Forge 1.16.3"
    ``` groovy
    repositories {
        maven { url 'https://repo.repsy.io/mvn/gandiber/geckolib' }
    }
    
    dependencies {
        implementation fg.deobf('{{ version_info_3.forge_1_16 }}')
    }
    ```
=== "Fabric 1.16.3"
    ```groovy
    repositories {
        maven { url 'https://repo.repsy.io/mvn/gandiber/geckolib' }
    }
    
    dependencies {
        modImplementation '{{ version_info_3.fabric_1_16 }}'
    }
    ```

!!! failure
    Do not put this in your buildscript section!
    
### Initializing GeckoLib

!!! info "Important!"
    Since GeckoLib is no longer an actual mod but rather a plain old library, you'll have to do a tiny bit more work to get it started.
    
    If you are using forge, simply put this line in your mod's constructor:
    
    ```java
    GeckoLib.initialize();
    ```
       
    !!! fabric
        For Fabric, you'll have to do this in your mod's `#!java onInitialize()` method.

## Installing the BB Plugin
To install the alpha blockbench plugin:

1. Close any GeckoLib project.
2. Open `Filter` -> `Plugins Menu`: 
3. If you have GeckoLib Animation Utils plugin installed already, uninstall it:
<img width="657" alt="Screen Shot 2020-10-10 at 11 08 25 AM" src="https://user-images.githubusercontent.com/110764/95662060-0441a200-0ae9-11eb-86c0-68df8943b40e.png">
4. Click `Load Plugin from URL`. 
    
    ![image](https://user-images.githubusercontent.com/110764/95662058-0146b180-0ae9-11eb-93c5-3a6887a05c2e.png)
    
5. Paste in this URL:

=== "Blockbench 3.7"
    ```
    https://rawcdn.githack.com/fadookie/geckolib-plugin/50a7fdc6a3d41a26d3f9e97e3c1875f24a1ce639/plugins/animation_utils.js
    ```
=== "Blockbench 3.6"
    ```
    https://rawcdn.githack.com/fadookie/geckolib-plugin/30e971d829202882fa11eb3c1e903eefcd3a286a/plugins/animation_utils.js
    ```

