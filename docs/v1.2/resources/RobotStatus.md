# Robot Status

Endpoints that provide real-time information on the robot's health and operational state, including connectivity, battery, emergency stop, mission, pose, navigation, and error codes.

-----------
## GetRobotStatus

Get the latest robot state.

Robot state includes connectivity and operational states.

### Request

The request takes no parameters.

##### JSON Request Example
=== "JSON"
    ```js
      {}
    ```

### Response

##### robot_state `RobotState`
The current robot state including connectivity, battery, emergency stop, mission, pose, error information, and velocity.

| Field | Message Type | Description |
|------|------|-------------|
| `connection` | [`RobotConnection`](#robotconnection) | Connection state of the robot. |
| `battery` | [`BatteryState`](#batterystate) | Battery state of the robot. |
| `emergency_stop` | [`EmergencyStopState`](#emergencystopstate) | Emergency stop state of the robot. |
| `mission` | [`MissionState`](Mission.md#missionstate) | Mission state of the robot. |
| `pose` | [`Pose`](LocalizationAndNavigation.md#pose) | Current pose of the robot. |
| `error_codes` | [`ErrorCodes`](#errorcodes) | Error codes returned by the robot. |
| `typed_status` | *oneof* | Robot type-specific state information. Only one type may be set at a time. |
| `twist` | [`Twist`](#twist) | Current linear and angular velocity of the robot. Omitted when not reported by the robot. |

##### Twist
Represents the current velocity of the robot in 2D (linear along the forward axis, angular around the vertical axis). Omitted when the robot does not report velocity.

| Field | Message Type | Description |
|------|------|-------------|
| `linear_velocity` | `float` | Current speed along the robot's forward axis, in m/s. Positive for forward, negative for reverse. |
| `angular_velocity` | `float` | Current rotation rate, in rad/s. Positive for clockwise when viewed from above. |

##### RobotConnection
Represents the online connection state between the cloud and the robot.

| Field | Message Type | Description |
|------|------|-------------|
| `state` | [`State`](#robotconnection-state-enum) *enum* | Current connection state of the robot. |

##### BatteryState
Represents the state of the robot's battery system.

| Field | Message Type | Description |
|------|------|-------------|
| `charge_percent` | `int32` | State of charge, from 0 (empty) to 100 (fully charged). |
| `state` | [`State`](#batterystate-state-enum) *enum* | High-level charging state of the battery. |
| `charge_method` | [`ChargeMethod`](#batterystate-chargemethod-enum) *enum* | Method by which the robot is being charged. |

##### EmergencyStopState
Represents the state of the robot's emergency stop system.

| Field | Message Type | Description |
|------|------|-------------|
| `emergency` | [`Emergency`](#emergencystopstate-emergency-enum) *enum* | Whether the software level emergency stop is engaged. |
| `button_pressed` | [`Emergency`](#emergencystopstate-emergency-enum) *enum* | Whether the physical emergency stop button is engaged. |

##### ErrorCodes
Represents the error codes returned by the robot.

| Field | Message Type | Description |
|------|------|-------------|
| `codes` | *repeated* [`ErrorCode`](#errorcode) | List of error codes currently active on the robot. See [SubscribeErrorCodes](#subscribeerrorcodes) for the `ErrorCode` and `Severity` definitions. |

##### typed_status `oneof`
Robot type-specific state information. Only one type may be set at a time.

| Field (*oneof*) | Message Type | Description |
|------|------|-------------|
| `servi_state` | [`ServiState`](#servistate) | Populated when the robot type is a Servi. |
| `carti_state` | [`CartiState`](#cartistate) | Populated when the robot type is a Carti. |

##### ServiState
Represents the set of robot states specifically for Servi robots.

| Field | Message Type | Description |
|------|------|-------------|
| `tray_states` | [`TrayStates`](Servi.md#traystates) | A collection of individual tray states from different trays. |

##### CartiState
Represents the set of robot states specifically for Carti robots.

| Field | Message Type | Description |
|------|------|-------------|
| `conveyor_state` | [`ConveyorState`](Carti.md#conveyorstate) *optional* | Conveyor state, only available for robots with a conveyor installed. |

#### (RobotConnection) State `enum`
| Name | Number | Description |
|------|--------|-------------|
| STATE_UNKNOWN | 0 | Default value. It means the `state` field is not returned. |
| STATE_CONNECTED | 1 | The robot is connected to Bear cloud services. |
| STATE_DISCONNECTED | 2 | The robot is offline or unreachable from the cloud. |

#### (BatteryState) State `enum`
| Name | Number | Description |
|------|--------|-------------|
| STATE_UNKNOWN | 0 | Default value. It means the `state` field is not returned. |
| STATE_CHARGING | 1 | Battery is currently charging. |
| STATE_DISCHARGING | 2 | Robot is not connected to a charger and is consuming battery power. |
| STATE_FULL | 3 | Battery is fully charged while connected to a charger; no additional energy is being stored. |

#### (BatteryState) ChargeMethod `enum`
| Name | Number | Description |
|------|--------|-------------|
| CHARGE_METHOD_UNKNOWN | 0 | Default value. It means the `charge_method` field is not returned. |
| CHARGE_METHOD_NONE | 1 | No charging method is currently active or applicable. |
| CHARGE_METHOD_WIRED | 2 | Charging via a wired connection. |
| CHARGE_METHOD_CONTACT | 3 | Charging via contact-based interface (e.g., docking station). |

#### (EmergencyStopState) Emergency `enum`
| Name | Number | Description |
|------|--------|-------------|
| EMERGENCY_UNKNOWN | 0 | Default value. It means the `emergency` field is not returned. |
| EMERGENCY_ENGAGED | 1 | Triggers an emergency stop. Overrides and sets navigation-related velocity command to 0 to the motor. |
| EMERGENCY_DISENGAGED | 2 | Wheels will resume acting upon software navigation commands. |

##### JSON Response Example
=== "JSON"
    ```js
      {
        "robotState": {
          "connection": {
            "state": "STATE_CONNECTED"
          },
          "battery": {
            "chargePercent": 85,
            "state": "STATE_CHARGING",
            "chargeMethod": "CHARGE_METHOD_CONTACT"
          },
          "emergencyStop": {
            "emergency": "EMERGENCY_DISENGAGED",
            "buttonPressed": "EMERGENCY_DISENGAGED"
          },
          "mission": {
            "missionId": "mission-123",
            "state": "STATE_RUNNING",
            "goals": [],
            "currentGoalIndex": 0
          },
          "pose": {
            "xMeters": 1.5,
            "yMeters": 2.8,
            "headingRadians": 0.78
          },
          "errorCodes": {
            "codes": [
              { "code": 1001, "severity": "SEVERITY_MEDIUM", "message": "Low battery warning" },
              { "code": 2042, "severity": "SEVERITY_HIGH", "message": "Camera connection lost" }
            ]
          },
          "twist": {
            "linearVelocity": 0.5,
            "angularVelocity": 0.0
          },
          "serviState": {
            "trayStates": {
              "trayStates": [
                {
                  "trayName": "top",
                  "loadState": "LOAD_STATE_LOADED",
                  "weightKg": 2.5,
                  "loadRatio": 0.8
                },
                {
                  "trayName": "middle",
                  "loadState": "LOAD_STATE_EMPTY",
                  "weightKg": 0.0,
                  "loadRatio": 0.0
                }
              ]
            }
          }
        }
      }
    ```

### Errors

| ErrorCode  | Description |
|------------|-------------|
| `INTERNAL` | Communication failure with the robot. |

-----------
## SubscribeRobotStatus
Streaming mode: [`event`](../../index.md#overview)

A [server side streaming RPC](https://grpc.io/docs/what-is-grpc/core-concepts/#server-streaming-rpc) endpoint to get the robot's connectivity and operational state.

Upon subscription, the latest robot state is sent immediately. Updates are streamed as the robot's state changes.

### Request

The request takes no parameters.

##### JSON Request Example
=== "JSON"
    ```js
      {}
    ```

### Response

##### metadata `EventMetadata`

| Field | Message Type | Description |
|------|------|-------------|
| `timestamp` | [`Timestamp`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/timestamp.proto) | The time when the event was recorded. |
| `sequence_number` | `int64` | An incremental sequence number generated by the robot.<br />The sequence number should never be negative and can be reset to 0.<br />i.e. sequence is valid if it is larger than the previous number or 0. |

##### robot_state `RobotState`
The current robot state including connectivity, battery, emergency stop, mission, pose, error information, and velocity.

| Field | Message Type | Description |
|------|------|-------------|
| `connection` | [`RobotConnection`](#robotconnection) | Connection state of the robot. |
| `battery` | [`BatteryState`](#batterystate) | Battery state of the robot. |
| `emergency_stop` | [`EmergencyStopState`](#emergencystopstate) | Emergency stop state of the robot. |
| `mission` | [`MissionState`](Mission.md#missionstate) | Mission state of the robot. |
| `pose` | [`Pose`](LocalizationAndNavigation.md#pose) | Current pose of the robot. |
| `error_codes` | [`ErrorCodes`](#errorcodes) | Error codes returned by the robot. |
| `typed_status` | *oneof* | Robot type-specific state information. Only one type may be set at a time. |
| `twist` | [`Twist`](#twist) | Current linear and angular velocity of the robot. Omitted when not reported by the robot. |

##### JSON Response Example
=== "JSON"
    ```json
    {
      "metadata": {
        "timestamp": "2025-04-01T17:05:00Z",
        "sequenceNumber": 211
      },
      "robotState": {
        "connection": {
          "state": "STATE_CONNECTED"
        },
        "battery": {
          "chargePercent": 75,
          "state": "STATE_CHARGING",
          "chargeMethod": "CHARGE_METHOD_CONTACT"
        },
        "emergencyStop": {
          "emergency": "EMERGENCY_DISENGAGED",
          "buttonPressed": "EMERGENCY_DISENGAGED"
        },
        "mission": {
          "missionId": "mission-456",
          "state": "STATE_RUNNING",
          "goals": [
            {
              "goal": {
                "pose": {
                  "xMeters": 5.2,
                  "yMeters": 3.1,
                  "headingRadians": 0.0
                }
              }
            }
          ],
          "currentGoalIndex": 1
        },
        "pose": {
          "xMeters": 2.1,
          "yMeters": 1.8,
          "headingRadians": 1.2
        },
        "errorCodes": {
          "codes": [
            { "code": 1001, "severity": "SEVERITY_MEDIUM", "message": "Low battery warning" },
            { "code": 2042, "severity": "SEVERITY_HIGH", "message": "Camera connection lost" }
          ]
        },
        "twist": {
          "linearVelocity": 0.3,
          "angularVelocity": 0.0
        }
      }
    }
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INTERNAL` | Internal server error occurred while processing the request. |

-----------
## SubscribeErrorCodes
Streaming mode: [`event`](../../index.md#overview)

A [server side streaming RPC](https://grpc.io/docs/what-is-grpc/core-concepts/#server-streaming-rpc) endpoint to subscribe to error codes returned by the robot.

Upon subscription, the latest known error codes are sent immediately. Updates are streamed when error codes change.

### Request

The request takes no parameters.

##### JSON Request Example
=== "JSON"
    ```js
      {}
    ```

### Response

##### error_codes `ErrorCodesWithMetadata`
The error codes reported by the robot, together with the event metadata.

| Field | Message Type | Description |
|------|------|-------------|
| `metadata` | [`EventMetadata`](#metadata-eventmetadata) | Metadata associated with the event. |
| `codes` | *repeated* [`ErrorCode`](#errorcode) | Error codes returned by the robot during the event. |

#### ErrorCode
Represents a single error reported by the robot.

| Field | Message Type | Description |
|------|------|-------------|
| `code` | `int32` | Integer code indicating the type of error. Does not indicate severity. |
| `severity` | [`Severity`](#errorcode-severity-enum) *enum* | Level of criticality of an error. |
| `message` | `string` | Message about the error e.g. "Up camera process error." |

#### (ErrorCode) Severity `enum`
| Name | Number | Description |
|------|--------|-------------|
| SEVERITY_UNKNOWN | 0 | Default value. It means the `severity` field is not returned. |
| SEVERITY_LOW | 1 | Low severity indicates an identified issue that is not visible. |
| SEVERITY_MEDIUM | 2 | Medium severity indicates an identified issue that does not block operation. |
| SEVERITY_HIGH | 3 | High severity indicates an identified issue that blocks operation. |

##### JSON Response Example
=== "JSON"
    ```js
    {
      "errorCodes": {
        "metadata": {
          "timestamp": "2025-04-01T17:10:00Z",
          "sequenceNumber": 215
        },
        "codes": [
          {
            "code": 1001,
            "severity": "SEVERITY_MEDIUM",
            "message": "Low battery warning"
          },
          {
            "code": 2042,
            "severity": "SEVERITY_HIGH",
            "message": "Camera connection lost"
          }
        ]
      }
    }
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INTERNAL` | Internal server error occurred while processing the request. |
