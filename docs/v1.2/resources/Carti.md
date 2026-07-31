These endpoints and their message types are only available for the Carti robot family. Attempting to run a Carti command on a non-Carti robot will result in an `INVALID_ARGUMENT` error.

------------
## CreateMission

Use the shared [`CreateMission`](Mission.md#createmission) / [`AppendMission`](Mission.md#appendmission) endpoints to send missions for Carti robots. Carti-specific missions are sent using the `carti_mission` payload below.

!!! note
    When sending a Carti mission, `carti.Feedback` and `CartiType` are returned in the [`SubscribeMissionStatus`](Mission.md#subscribemissionstatus) response.

##### carti_mission `carti.Mission`
Use the field `carti_mission` to create and send a mission.

| Field (*oneof*) | Message Type | Description |
|------------|-------------| ---|
|`traverse_mission`   |[`TraverseMission`](#traversemission)	| Create a carti mission of type `Traverse`. |
|`traverse_patrol_mission`	|[`TraversePatrolMission`](#traversepatrolmission)| Create a carti mission of type `TraversePatrol`. |
|`navigate_mission`	|[`NavigateMission`](#navigatemission)| Create a carti mission of type `Navigate`. |
|`navigate_auto_mission`	|[`NavigateAutoMission`](#navigateautomission)| Create a carti mission of type `NavigateAuto`. |

#### TraverseMission
A traverse mission that navigates to one or more goals, stopping at each for a set amount of time or until directed to continue.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| a list of `Goal` |
|`params`|`TraverseParams` <br />`optional`|  ***There is no param defined in this API version.*** |

#### TraversePatrolMission
A traverse patrol mission that navigates to one or more goals and continuously loops through the goals, stopping at each for a set amount of time or until directed to continue.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| a list of `Goal` |
|`params`|`TraversePatrolParams` <br />`optional`|  ***There is no param defined in this API version.*** |

#### NavigateMission
A mission consisting of a single, explicitly defined goal.

| Field | Message Type | Description |
|------|------|-------------|
|`goal`| [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| A single `Goal` |

#### NavigateAutoMission
A mission that automatically selects the first unoccupied and unclaimed goal from the provided list, preferring goals with lower index values.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| a list of `Goal` |

#### Carti workflow payloads (CreateMissionWorkflow) {#carti-workflow-payloads-createmissionworkflow}

!!! Note "CreateMissionWorkflow"
    When using [CreateMissionWorkflow](Mission.md#createmissionworkflow), set one of the following under `cartiWorkflow`. Each type carries only goals with no params.

    | Field (*oneof*) | Message Type | Description |
    |----------------|--------------|-------------|
    | `traverse` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |
    | `traverse_patrol` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |

------------
## GetConveyorIndex

Retrieves the configured conveyor indexes for the robot. Indexes represent logical positions, not physical installation:

- **Carti 100 (vertical)**: INDEX_1ST = uppermost conveyor
- **Carti 600 (horizontal)**: INDEX_1ST = front facing conveyor

### Request

*(No fields defined)*

##### JSON Request Example
=== "JSON"
    ```js
      {}
    ```

### Response

##### indexes `repeated int32`
List of available conveyor indexes for this robot.

##### JSON Response Example
=== "JSON"
    ```js
      {
        "indexes": [1, 2]
      }
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INVALID_ARGUMENT` | This command is being sent to a non-Carti family robot. |
| `INTERNAL` | Internal server error occurred while processing the request. |

-----------
## SubscribeConveyorStatus
Streaming mode: [`event`](../../index.md#overview)

A [server side streaming RPC](https://grpc.io/docs/what-is-grpc/core-concepts/#server-streaming-rpc) endpoint to get conveyor status updates for the robot.

Upon subscription, the latest known conveyor states are sent immediately. Updates are streamed when any conveyor state changes.

### Request

*(No fields defined)*

##### JSON Request Example
=== "JSON"
    ```js
      {}
    ```

### Response

##### states `repeated ConveyorState`
State of all conveyors on the robot.

##### ConveyorState
Represents the state of a single conveyor.

| Field | Message Type | Description |
|------|------|-------------|
| `index` | `int32` | Unique identifier for the conveyor (logical position). |
| `operation_state` | [`OperationState`](#operationstate-enum) *enum* | The current state of the conveyor motor. |
| `payload_state` | [`PayloadState`](#payloadstate-enum) *enum* | The current state of the payload on the conveyor. |
| `health_state` | [`HealthState`](#healthstate-enum) *enum* | The current health of the conveyor. |
| `installation_state` | [`InstallationState`](#installationstate-enum) *enum* | Current installation state of the conveyor. |

#### OperationState `enum`
| Name | Number | Description |
|------|--------|-------------|
| OPERATION_STATE_UNKNOWN | 0 | Default value. It means the `operation_state` field is not returned. |
| OPERATION_STATE_ROLLING | 1 | The conveyor's motor is rolling in any direction. |
| OPERATION_STATE_STOP | 2 | The conveyor's motor is stopped. |

#### PayloadState `enum`
| Name | Number | Description |
|------|--------|-------------|
| PAYLOAD_STATE_UNKNOWN | 0 | Default value. It means the `payload_state` field is not returned. |
| PAYLOAD_STATE_LOADED | 1 | The conveyor has a payload. |
| PAYLOAD_STATE_EMPTY | 2 | The conveyor is empty. |

#### HealthState `enum`
| Name | Number | Description |
|------|--------|-------------|
| HEALTH_STATE_UNKNOWN | 0 | Default value. It means the `health_state` field is not returned. |
| HEALTH_STATE_OK | 1 | The conveyor is installed and functioning. |
| HEALTH_STATE_ERROR | 2 | The conveyor is not functioning. |

#### InstallationState `enum`
| Name | Number | Description |
|------|--------|-------------|
| INSTALLATION_STATE_UNKNOWN | 0 | Default value. It means the `installation_state` field is not returned. |
| INSTALLATION_STATE_INSTALLED | 1 | The conveyor is installed. |
| INSTALLATION_STATE_NOT_INSTALLED | 2 | The conveyor is not installed. |

##### JSON Response Example
=== "JSON"
    ```js
    {
      "states": [
        {
          "index": 1,
          "operationState": "OPERATION_STATE_ROLLING",
          "payloadState": "PAYLOAD_STATE_LOADED",
          "healthState": "HEALTH_STATE_OK",
          "installationState": "INSTALLATION_STATE_INSTALLED"
        },
        {
          "index": 2,
          "operationState": "OPERATION_STATE_STOP",
          "payloadState": "PAYLOAD_STATE_EMPTY",
          "healthState": "HEALTH_STATE_OK",
          "installationState": "INSTALLATION_STATE_INSTALLED"
        }
      ]
    }
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INVALID_ARGUMENT` | This command is being sent to a non-Carti family robot. |
| `INTERNAL` | Internal server error occurred while processing the request. |

-----------
## ControlConveyor

Controls conveyor motor operations for the specified conveyor indexes. This call allows manual control of conveyor motors for clockwise/counter-clockwise rotation or stop commands.

### Request

##### commands `repeated ConveyorMotorCommand` `required`
List of conveyor motor commands to execute.

##### ConveyorMotorCommand
| Field | Message Type | Description |
|------|------|-------------|
| `index` | `int32` | The target conveyor to control. |
| `command` | [`CommandConveyorMotor`](#commandconveyormotor-enum) *enum* | The motor command to execute. |

#### CommandConveyorMotor `enum`
| Name | Number | Description |
|------|--------|-------------|
| COMMAND_CONVEYOR_MOTOR_UNKNOWN | 0 | Default value. This should never be used explicitly. |
| COMMAND_CONVEYOR_MOTOR_STOP | 1 | Stop the conveyor. |
| COMMAND_CONVEYOR_MOTOR_CW | 2 | Rotate the conveyor clockwise. |
| COMMAND_CONVEYOR_MOTOR_CCW | 3 | Rotate the conveyor counter-clockwise. |

##### JSON Request Example
=== "JSON"
    ```js
      {
        "commands": [
          {
            "index": 1,
            "command": "COMMAND_CONVEYOR_MOTOR_CW"
          },
          {
            "index": 2,
            "command": "COMMAND_CONVEYOR_MOTOR_STOP"
          }
        ]
      }
    ```

### Response

*(No fields defined)*

##### JSON Response Example
=== "JSON"
    ```js
      {}
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INVALID_ARGUMENT` | This command is being sent to a non-Carti family robot, or one or more conveyor indexes are not installed on the robot. |
| `FAILED_PRECONDITION` | The robot is in an error state that prevents conveyor control. |
| `INTERNAL` | Internal server error occurred while processing the request. |
