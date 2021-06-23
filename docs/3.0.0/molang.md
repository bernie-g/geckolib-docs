# Molang
[Molang](https://minecraft.gamepedia.com/Bedrock_Edition_beta_MoLang_documentation) is a simple but powerful expression-based language that can create very complex and powerful animations. It has a learning curve, but once you master it it becomes a very useful tool in your animation toolset. 

Molang allows you to type math expressions into keyframe values instead of just numbers. For example, you could type `5 * 4` into a scale keyframe, and it would evaluate to 20. You can also do much more complex things using math functions like `math.sin`, `math.cos`, and `math.abs`. Read about all the math functions on the [bedrock wiki](https://bedrock.dev/docs/stable/MoLang#Math%20Functions). 

GeckoLib 3.0.0 supports Molang, and has several useful queries. 

## Supported Queries
- `query.anim_time`
- `query.actor_count`
- `query.time_of_day`
- `query.moon_phase`
- `query.distance_from_camera`
- `query.is_on_ground`
- `query.is_in_water`
- `query.is_in_water_or_rain`
- `query.health`
- `query.max_health`
- `query.is_on_fire`
- `query.on_fire_time`
- `query.ground_speed`
- `query.yaw_speed`

## Examples
??? example "Horizontal sine"
    **Position:**
    
    In this example, the 200 is used as a modifier for the speed of the cube, and the 10 is used to control how far it moves.
    
    - X: `0`
    - Y: `0`
    - Z: `math.sin(query.anim_time * 200) * 10`
    
    ![!circle](https://github.com/bernie-g/geckolib-docs/blob/master/docs/images/gifs/molang-examples/horizontal-sine.gif?raw=true){:width=350}

??? example "Circle"
    **Position:**

    - X: `0`
    - Y: `math.cos(query.anim_time * 200) * 10`
    - Z: `math.sin(query.anim_time * 200) * 10`
    
    ![!circle](https://github.com/bernie-g/geckolib-docs/blob/master/docs/images/gifs/molang-examples/circle.gif?raw=true){:width=350}

??? example "Bouncy Cube"
    **Position:**
   
    - X: `0`
    - Y: `math.abs(math.sin(query.anim_time * 500) * math.exp(-0.5 * query.anim_time)) * 10`
    - Z: `0`

    ![!cube](https://github.com/bernie-g/geckolib-docs/blob/master/docs/images/gifs/molang-examples/bouncy-cube.gif?raw=true){:width=350}


??? example "Crystal"
    **Rotation:**

    - X: `math.sin(query.anim_time * 100) * 150`
    - Y: `math.cos(query.anim_time * 100) * 150`
    - Z: `math.sin(query.anim_time * 100) * 150`
    
    ![!crystal](https://github.com/bernie-g/geckolib-docs/blob/master/docs/images/gifs/molang-examples/crystal.gif?raw=true){:width=350}
    
    
??? example "Arc"
    **Position:**

    To arc a bone between two points, see this [article](https://gamedev.stackexchange.com/questions/157642/moving-a-2d-object-along-circular-arc-between-two-points)
    
    In this example, the bone is moving between from (-10, 0) to (10, 0) and uses (0, 15) as it's height control point. It will never reach (0, 15).

    - X: `Math.lerp(-10, 10, query.anim_time)`
    - Y: `Math.lerp(Math.lerp(0, temp.control_height, query.anim_time), Math.lerp(temp.control_height, 0, query.anim_time), query.anim_time)`
    - Z: `0`
    
    **Preview Variables:**
    ```
    temp.control_height = 50;
    ```
    
    ![!arc](https://github.com/bernie-g/geckolib-docs/blob/master/docs/images/gifs/molang-examples/arc.gif?raw=true){:width=350}
        
??? example "Human Idle"
    Left Arm:
    
    - X: `Math.cos(query.anim_time * 150 +75) * -15`
    - Y: `0`
    - Z: `0`
        
    Right Arm (Rotation):
    
    - X: `Math.cos(query.anim_time * 150 +25) * 15`
    - Y: `0`
    - Z: `0`
    
    Body (Position):
    
    - X: `0`
    - Y: `Math.cos(query.anim_time * 100) * 0.5 -0.5`
    - Z: `0`
    
    Head (Rotation):
    
    - X: `Math.cos(query.anim_time * 100 -50) * -5`
    - Y: `0`
    - Z: `0`
    
    ![!idle](https://github.com/bernie-g/geckolib-docs/blob/master/docs/images/gifs/molang-examples/idle_anim.gif?raw=true){:width=350}
    
    Try out this model [here!](https://drive.google.com/uc?id=1KjG6O0A_Oh7yDlh42vYwMYekaR6CilHU)
    
??? example "Controlling Animation Speed"
    **Position:**

    To control the speed of an animation, define your own variable. In this example, we'll name it `anim_speed`. Then, either set it to a predefined value, like 2. Then in code you can set this value using `#!java GeckoLibCache.getInstance().parser.setValue("anim_speed", 2);` Simply set this in your renderer, and it will get updated every frame before being animated. In this example, the anim_speed variable is updated dynamically to match the `query.anim_time` field, effectively making the animation speed up over time.
    
    - X: `Math.lerp(-30, 30, query.anim_time * anim_speed)`
    - Y: `Math.lerp(Math.lerp(0, temp.control_height, query.anim_time * anim_speed), Math.lerp(temp.control_height, 0, query.anim_time * anim_speed), query.anim_time * anim_speed)`
    - Z: `0`
    
    **Preview Variables:**
    ```
    temp.control_height = 80;
    anim_speed = query.anim_time / 2;
    ```
    
    ![!arc](https://github.com/bernie-g/geckolib-docs/blob/master/docs/images/gifs/molang-examples/anim_speed.gif?raw=true){:width=350}
    
Read about what all these queries do on the [wiki](https://bedrock.dev/docs/stable/MoLang#List%20of%20Entity%20Queries).

If GeckoLib is missing a query that you need, ask us in the discord.
