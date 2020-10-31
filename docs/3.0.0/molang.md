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
#### Horizontal sine
Position:

- X: `0`
- Y: `0`
- Z: `math.sin(query.anim_time * 200) * 10`

<img width="350" alt="Horizontal sine example" src="..\..\images\gifs\molang-examples\horizontal-sine.gif">

#### Circle
Position:

- X: `0`
- Y: `math.cos(query.anim_time * 200) * 10`
- Z: `math.sin(query.anim_time * 200) * 10`

<img width="350" alt="Horizontal sine example" src="..\..\images\gifs\molang-examples\circle.gif">

#### Bouncy cube
Position:

- X: `0`
- Y: `math.abs(math.sin(query.anim_time * 500) * math.exp(-0.5 * query.anim_time)) * 10`
- Z: `0`

<img width="350" alt="Horizontal sine example" src="..\..\images\gifs\molang-examples\bouncy-cube.gif">

#### Crystal
Rotation:

- X: `math.sin(query.anim_time * 100) * 150`
- Y: `math.cos(query.anim_time * 100) * 150`
- Z: `math.sin(query.anim_time * 100) * 150`

<img width="350" alt="Horizontal sine example" src="..\..\images\gifs\molang-examples\crystal.gif">

Read about what all these queries do on the [wiki](https://bedrock.dev/docs/stable/MoLang#List%20of%20Entity%20Queries).

If GeckoLib is missing a query that you need, ask us in the discord.