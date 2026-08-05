# Localization & Navigation

Message types for navigation goals and robot localization. A [`Goal`](#goal) specifies a mission target; [`Pose`](#pose) and [`LocalizationState`](#localizationstate-enum) are reported in [Robot Status](RobotStatus.md#getrobotstatus).

------------

### Pose
Represents the robot's pose on the map.

| Field | Type | Description |
|-------|------|-------------|
| `x_meters` | `float` | X-coordinate in meters within the map. |
| `y_meters` | `float` | Y-coordinate in meters within the map. |
| `heading_radians` | `float` | The heading of the robot in radians. Ranges from -π to π, where 0.0 points along the positive x-axis. |

##### Example
=== "JSON"
    ```json
      {
        "xMeters": 2.5,
        "yMeters": 3.0,
        "headingRadians": 1.57
      }
    ```

### Goal
Represents a target destination or pose for the robot to navigate to. Exactly one of the fields below may be set.

| Field (*oneof*) | Type | Description |
|-----------------|------|-------------|
| `destination_id` | `string` | Unique identifier for the destination. See [`Destination`](LocationsAndMaps.md#destination). |
| `pose` | [`Pose`](#pose) | Pose of the robot on the map. |

##### Example
=== "JSON"
    ```json
      {
        "pose": {
          "xMeters": 2.5,
          "yMeters": 3.0,
          "headingRadians": 1.57
        }
      }
    ```

### LocalizationState
Represents the current state of the localization process.

| Field | Type | Description |
|-------|------|-------------|
| `state` | [`State`](#localizationstate-enum) `enum` | Current state of the localization process. |

#### LocalizationState `enum`

| Name | Number | Description |
|------|--------|-------------|
| `STATE_UNKNOWN` | 0 | Default value. |
| `STATE_FAILED` | 1 | Localization failed. |
| `STATE_SUCCEEDED` | 2 | Localization completed successfully. |
| `STATE_LOCALIZING` | 3 | The robot is actively attempting to localize. |
| `STATE_MISLOCALIZED` | 4 | The robot has lost its pose and needs relocalization. |

##### Example
=== "JSON"
    ```json
      {
        "state": "STATE_SUCCEEDED"
      }
    ```
