# Installation
## Installing the Library
To install the actual geckolib forge library, insert this dependency snippet into your build.gradle. 

=== "Forge 1.15.2"
    ```groovy
    repositories{
        maven { url 'https://dl.cloudsmith.io/public/geckolib3/geckolib/maven/'' }
    }
    dependencies{
        compile fg.deobf('{{ version_info_3.forge_1_15 }}')
    }
    ```
=== "Forge 1.16.5"
    ``` groovy
    repositories {
        maven { url 'https://dl.cloudsmith.io/public/geckolib3/geckolib/maven/'' }
    }
    
    dependencies {
        implementation fg.deobf('{{ version_info_3.forge_1_16 }}')
    }
    ```
=== "Fabric 1.16.5"
    ```groovy
    repositories {
        maven { url 'https://dl.cloudsmith.io/public/geckolib3/geckolib/maven/'' }
    }
    
    dependencies {
        modImplementation '{{ version_info_3.fabric_1_16 }}'
    }
    ```
=== "Forge 1.12.2"
    ```groovy
    repositories{
        maven { url 'https://dl.cloudsmith.io/public/geckolib3/geckolib/maven/'' }
    }
    dependencies{
        deobfCompile('{{ version_info_3.forge_1_12 }}')
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
To install the blockbench plugin, simply go to `File` -> `Plugins` -> `Available` -> `Install GeckoLib Animation Utils`.
