# Mission

Missions are atomic units of behavior that define a robot's high-level actions.
This API enables users to control robot behavior—from simple navigation
tasks to complex, conditional workflows.

## Message Types

### Destination
Point on the map.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`destination_id`   |`string`	| The ID of the destination. |

##### Example
=== "JSON"
    ```json
      {
        "destinationId": "pickup_zone"
      }
    ```

### Zone
Area on the map with one or more points where any one point is a valid goal for the robot.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`zone_id`   |`string`	| The ID of the zone. |

##### Example
=== "JSON"
    ```json
      {
        "zoneId": "delivery_zone_1"
      }
    ```

### Goal
Place on the map where the robot can be set to navigate towards.

| Field (*oneof*) | Message Type | Description |
|------------|-------------| ---|
|`destination`   |[`Destination`](#destination)	| A point on the map identified by destination ID. |
|`zone`	|[`Zone`](#zone) | An area on the map identified by zone ID. |
|`pose`	|[`Pose`](Robot.md#pose)	| A specific pose (x, y, heading) on the map. |

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

### Mission
Represents a mission.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`type`   |[`Type`](#missiontype-enum) *enum*	| The type of the mission. |
|`goals`	|*repeated* [`Goal`](#goal)	| The list of goals or destinations for the mission. |
|`override_params`	|[`MissionParams`](#missionparams)	| Override parameters for the mission settings, allowing specific configuration for this mission instance. |

#### Mission.Type `enum`

| Name               | Number | Description                                                                |
|--------------------|--------|----------------------------------------------------------------------------|
| `TYPE_UNKNOWN`       | 0      | Default value, indicates an unknown or unspecified mission type.           |
| `TYPE_ONEOFF`        | 1      | A single-goal mission.                                                     |
| `TYPE_ONEOFF_AUTO`   | 2      | An automated single-goal mission that selects the best available goal from a list.     |
| `TYPE_TRAVERSE`      | 3      | A mission involving multiple destinations until a condition, such as weight limit, is met. |
| `TYPE_LOOP`          | 4      | A mission that repeatedly visits multiple destinations until a condition, such as weight limit, is met. |
| `TYPE_WAIT`          | 5      | A mission that remains at a specific location until triggered by an external event, such as a button press. |

##### Example
=== "JSON"
    ```json
      {
        "type": "TYPE_ONEOFF",
        "goals": [
          {
            "pose": {
              "xMeters": 2.5,
              "yMeters": 3.0,
              "headingRadians": 1.57
            }
          }
        ]
      }
    ```

### MissionCommand
Action to update a current mission.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`mission_id`   |`string`	| The ID of the mission to control. |
|`command`	|[`Command`](#missioncommandcommand-enum) *enum*	| Command to update the state of an active mission. |

#### MissionCommand.Command `enum`

| Name                   | Number | Description                                      |
|------------------------|--------|--------------------------------------------------|
| COMMAND_UNKNOWN        | 0      | Default value. This should never be used explicitly. <br/> It means the `command` field is not set|
| COMMAND_CANCEL         | 1      | Cancel this mission.                             |
| COMMAND_PAUSE          | 2      | Pause this mission.                              |
| COMMAND_RESUME         | 3      | Resume a paused mission.                         |
| COMMAND_FINISH         | 4      | Mark the mission as completed.                   |

##### Example
=== "JSON"
    ```json
      {
        "missionId": "f842c8ac-62de-412e-90fb-bf37022db2f4",
        "command": "COMMAND_PAUSE"
      }
    ```

### MissionParams
Override parameters for the mission settings.

| Field (*oneof*) | Message Type | Description |
|------------|-------------| ---|
|`traverse_params`   |[`TraverseParams`](#missionparamstraverseparams)	| Parameters for traverse missions. |
|`loop_params`	|[`LoopParams`](#missionparamsloopparams)	| Parameters for loop missions. |

#### MissionParams.Mode `enum`

| Name | Number | Description |
|------|--------|-------------|
| MODE_UNKNOWN | 0 | Default value. |
| MODE_DEFAULT | 1 | Default mode. |
| MODE_BUSSING | 2 | Bussing mode. |

#### MissionParams.TraverseParams

| Field  | Message Type | Description |
|------------|-------------| ---|
|`mode`   |[`Mode`](#missionparamsmode-enum) *enum*	| The mode for the traverse mission. |

##### Example
=== "JSON"
    ```json
      {
        "traverseParams": {
          "mode": "MODE_DEFAULT"
        }
      }
    ```

#### MissionParams.LoopParams

| Field  | Message Type | Description |
|------------|-------------| ---|
|`mode`   |[`Mode`](#missionparamsmode-enum) *enum*	| The mode for the loop mission. |

##### Example
=== "JSON"
    ```json
      {
        "loopParams": {
          "mode": "MODE_BUSSING"
        }
      }
    ```

### MissionState
Represents a mission state.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`mission_id`   |`string`	| Unique identifier for the mission. |
|`state`	|[`State`](#missionstatestate-enum) *enum*	| Current lifecycle state of the mission. |
|`goals`	|*repeated* [`Goal`](#goal)	| All goals for a given mission. |
|`current_goal_index`	|`int32`	| Index of the currently active goal in the goals list. |
|`navigation_status`	|[`NavigationStatus`](#missionstatenavigationstatus-enum) *enum*	| The current navigation status of the mission, reflecting the robot's progress toward or at its destination. |
|`feedback`	|[`Any`](https://protobuf.dev/reference/protobuf/google.protobuf/#any)	| Mission-specific feedback data. |

#### MissionState.State `enum`

| Name                   | Number | Description                                      |
|------------------------|--------|--------------------------------------------------|
| STATE_UNKNOWN          | 0      | Default value. It means the `state` field is not returned. |
| STATE_DEFAULT          | 1      | Initial state when no mission has been run (e.g., feedback is empty).    |
| STATE_RUNNING          | 2      | The mission is actively running.                 |
| STATE_PAUSED           | 3      | The mission is paused.                          |
| STATE_CANCELED         | 4      | The mission was canceled before completion.    |
| STATE_SUCCEEDED        | 5      | The mission completed successfully.           |
| STATE_FAILED           | 6      | The mission encountered an error or failure.          |

#### MissionState.NavigationStatus `enum`

| Name                   | Number | Description                                      |
|------------------------|--------|--------------------------------------------------|
| NAVIGATION_STATUS_UNKNOWN | 0 | Default value, indicates an unknown or undefined navigation status. |
| NAVIGATION_STATUS_FINISHED | 1 | Indicates that the robot has successfully arrived at its goal. |
| NAVIGATION_STATUS_FAILED | 2 | Indicates that the robot failed to reach its goal. |
| NAVIGATION_STATUS_STUCK | 3 | Indicates that the robot is temporarily stuck but has not yet failed. |
| NAVIGATION_STATUS_DOCKING | 4 | Indicates that the robot is in the process of docking at a charger or station. |
| NAVIGATION_STATUS_UNDOCKING | 5 | Indicates that the robot is in the process of undocking at a charger or station. |
| NAVIGATION_STATUS_NAVIGATING | 6 | Indicates that the robot is navigating to a destination but is not currently docking or undocking. |

##### Example
=== "JSON"
    ```json
      {
        "missionId": "d6637a14-5f6b-43f6-bd86-cc1871a8322e",
        "state": "STATE_RUNNING",
        "goals": [
          {
            "pose": {
              "xMeters": 4.2,
              "yMeters": 7.8,
              "headingRadians": 1.57
            }
          }
        ],
        "currentGoalIndex": 1,
        "navigationStatus": "NAVIGATION_STATUS_NAVIGATING"
      }
    ```

### GenericGoal
A flexible goal definition used in generic missions.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`goal_type`   |`string`	| The type of the goal (e.g., "Pickup", "Dropoff"). |
|`parameters`	|`map<string, string>`	| Any optional parameters for the goal in a key-value format. |

##### Example
=== "JSON"
    ```json
      {
        "goalType": "Pickup",
        "parameters": {
          "location": "kitchen",
          "priority": "high"
        }
      }
    ```

### GenericMission
A generic mission that supports string-typed mission names and flexible goals.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`type`   |`string`	| The type of the generic mission (e.g., "Traverse", "Loop"). |
|`goals`	|*repeated* [`GenericGoal`](#genericgoal)	| A list of generic goals associated with the mission. Each goal includes its type and other relevant information. |
|`override_params`	|`map<string, string>`	| A map of key-value pairs for overriding default mission parameters. This provides flexibility to customize the mission's behavior. |

##### Example
=== "JSON"
    ```json
      {
        "type": "Traverse",
        "goals": [
          {
            "goalType": "Pickup",
            "parameters": {
              "location": "kitchen"
            }
          },
          {
            "goalType": "Dropoff",
            "parameters": {
              "location": "table_5"
            }
          }
        ],
        "overrideParams": {
          "maxSpeed": "0.8"
        }
      }
    ```
