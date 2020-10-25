# Molang
[Molang](https://minecraft.gamepedia.com/Bedrock_Edition_beta_MoLang_documentation) is a simple but powerful expression-based language that can create very complex and powerful animations. It has a learning curve, but once you master it it becomes a very useful tool in your animation toolset. 

Molang allows you to type math expressions into keyframe values instead of just numbers. For example, you could type `5 * 4` into a scale keyframe, and it would evaluate to 20. You can also do much more complex things using math functions like `sin`, `cos`, and `abs`. Read about all the math functions on the [bedrock wiki](https://bedrock.dev/docs/stable/MoLang#Math%20Functions). 

GeckoLib 3.0.0 supports molang, and has several useful queries. 

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


Read about what all these queries do on the [wiki](https://bedrock.dev/docs/stable/MoLang#List%20of%20Entity%20Queries).

If GeckoLib is missing a query that you need, ask us in the discord.