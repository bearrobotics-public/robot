# Zones

Zones messages provide information about parameter zones on the map.

## Message Types

### ParameterZone
Areas on the map that have specific parameters.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`zone_id`   |`string`	| The ID of the zone. |
|`points`	|*repeated* [`Point2D`](../common/Math.md#point2d)	| Polygon defining the zone. The minimum number of points is 3. |
|`direction_zone`	|[`DirectionZone`](#directionzone)	| Direction zone parameters. |
|`exclusive_zone`	|[`ExclusiveZone`](#exclusivezone)	| Exclusive zone parameters. |
|`ramp_zone`	|[`RampZone`](#rampzone)	| Ramp zone parameters. |
|`sound_zone`	|[`SoundZone`](#soundzone)	| Sound zone parameters. |
|`speed_zone`	|[`SpeedZone`](#speedzone)	| Speed zone parameters. |

##### Example
=== "JSON"
    ```json
      {
        "zoneId": "zone_001",
        "points": [
          {"x": 0.0, "y": 0.0},
          {"x": 1.0, "y": 0.0},
          {"x": 1.0, "y": 1.0},
          {"x": 0.0, "y": 1.0}
        ],
        "speedZone": {
          "maxSpeedMPerSec": 0.5
        }
      }
    ```

### DirectionZone
Defines a direction vector for the zone.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`heading_radians`   |`float`	| The direction vector's angle in radians, relative to the map's origin and measured from the x-axis. |
|`magnitude`	|`int32`	| The magnitude of the direction vector. Typically set to 254 for hard direction zones. The larger the magnitude, the more the robot will try to align with the direction. |

##### Example
=== "JSON"
    ```json
      {
        "headingRadians": 1.57,
        "magnitude": 254
      }
    ```

### ExclusiveZone
Defines an exclusive zone that limits the number of robots.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`max_robots`   |`int32`	| Maximum number of robots allowed to enter the zone. |

##### Example
=== "JSON"
    ```json
      {
        "maxRobots": 2
      }
    ```

### RampZone
Indicates a ramp zone. No additional parameters are required.

*(No fields defined)*

##### Example
=== "JSON"
    ```json
      {}
    ```

### SoundZone
Indicates a sound zone. No additional parameters are required.

*(No fields defined)*

##### Example
=== "JSON"
    ```json
      {}
    ```

### SpeedZone
Defines a speed limit for the zone.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`max_speed_m_per_sec`   |`float`	| Speed limit in the zone. |

##### Example
=== "JSON"
    ```json
      {
        "maxSpeedMPerSec": 0.5
      }
    ```
