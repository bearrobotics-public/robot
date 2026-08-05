# Localization

Localization allows the robot to determine its position and orientation on the map.

## Message Types

### LocalizationGoal
The robot must be placed within a 5x5 meter window from the localization goal.

| Field (*oneof*) | Message Type | Description |
|------------|-------------| ---|
|`pose`   |[`Pose`](Robot.md#pose)	| The target pose for localization. |

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
Represents the current localization state.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`state`   |[`State`](#localizationstatestate-enum) *enum*	| The current localization state. |

#### LocalizationState.State `enum`

| Name             | Number | Description                                               |
|------------------|--------|-----------------------------------------------------------|
| `STATE_UNKNOWN`    | 0      | Default value. It means the `state` field is not returned. |
| `STATE_PREEMPTED`  | 1      | Happens when the localization process is preempted before completion. |
| `STATE_FAILED`     | 2      | The localization process failed. |
| `STATE_SUCCEEDED`  | 3      | The localization process completed successfully. |
| `STATE_LOCALIZING` | 4      | The robot is actively performing localization.            |

##### Example
=== "JSON"
    ```json
      {
        "state": "STATE_LOCALIZING"
      }
    ```
