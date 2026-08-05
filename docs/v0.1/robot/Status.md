# Status

Status messages provide information about the robot's current state, including battery, emergency stop, and operation status.

## Message Types

### BatteryState
Represents the state of the robot's battery system.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`charge_percent`   |`int32`	| State of charge from 0 (battery empty) to 100 (battery full). |
|`state`	|[`State`](#batterystatestate-enum) *enum*	| The current battery state. |
|`charge_method`	|[`ChargeMethod`](#batterystatechargemethod-enum) *enum*	| The method used for charging. |

#### BatteryState.State `enum`

| Name               | Number | Description                                                                 |
|--------------------|--------|-----------------------------------------------------------------------------|
| `STATE_UNKNOWN`      | 0      | Default value. It means the `state` field is not returned. |
| `STATE_CHARGING`     | 1      | The battery is currently charging. |
| `STATE_DISCHARGING`  | 2      | Robot is not connected to the charger and is draining energy from the battery. |
| `STATE_FULL`         | 3      | While connected to the charger, the battery is fully charged, no more energy can be stored into the battery. |

#### BatteryState.ChargeMethod `enum`

| Name                     | Number | Description |
|--------------------------|--------|-------------|
| `CHARGE_METHOD_UNKNOWN`    | 0      | Default value. |
| `CHARGE_METHOD_NOT_CHARGING` | 1    | The robot is not charging. |
| `CHARGE_METHOD_WIRED`      | 2      | The robot is charging via a wired connection. |
| `CHARGE_METHOD_WIRELESS`   | 3      | The robot is charging wirelessly (inductive or contact charger). |

##### Example
=== "JSON"
    ```json
      {
        "chargePercent": 85,
        "state": "STATE_CHARGING",
        "chargeMethod": "CHARGE_METHOD_WIRELESS"
      }
    ```

### EmergencyStopState
Represents the emergency stop state.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`emergency`   |[`Emergency`](#emergencystopstateemergency-enum) *enum*	| The emergency stop state. |

#### EmergencyStopState.Emergency `enum`

| Name | Number | Description |
|------|--------|-------------|
| `EMERGENCY_UNKNOWN` | 0 | Default value. |
| `EMERGENCY_ENGAGED` | 1 | Triggers an emergency stop. Overrides and sets navigation-related velocity command to 0 to the motor. |
| `EMERGENCY_DISENGAGED` | 2 | Wheels will resume acting upon software navigation commands. |

##### Example
=== "JSON"
    ```json
      {
        "emergency": "EMERGENCY_DISENGAGED"
      }
    ```

### OperationState
High-level representation of various operating behavior on the robot.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`system`   |[`System`](#operationstatesystem-enum) *enum*	| Represents overall health of the robot. (e.g. whether a hardware device is reachable) |
|`system_message`	|`string`	| Optional message for human readability. Could be empty. |
|`emergency`	|[`Emergency`](#operationstateemergency-enum) *enum*	| Represents the robot's current emergency state. Based on the physical e-stop button and software e-stop. |
|`emergency_message`	|`string`	| Optional message for human readability. Could be empty. |
|`charging`	|[`Charging`](#operationstatecharging-enum) *enum*	| Represents the robot's current charging behavior. Certain charging methods may limit what actions the robot is able to take. |
|`mission`	|[`Mission`](#operationstatemission-enum) *enum*	| Represents any mission-related action being in progress on the robot. |

#### OperationState.System `enum`

| Name | Number | Description |
|------|--------|-------------|
| `SYSTEM_UNKNOWN` | 0 | Default value. |
| `SYSTEM_OK` | 1 | No faults are detected in system operation. |
| `SYSTEM_ERROR` | 2 | One or more errors are present on the robot. This could be software-related or hardware communication. The robot may not be operable depending on the nature of the error. |

#### OperationState.Emergency `enum`

| Name | Number | Description |
|------|--------|-------------|
| `EMERGENCY_UNKNOWN` | 0 | Default value. |
| `EMERGENCY_ENGAGED` | 1 | Either robot's physical e-stop button is pressed or a software e-stop has been triggered. Wheels are locked in a halted state and will not respond to software navigation commands. |
| `EMERGENCY_DISENGAGED` | 2 | Both physical e-stop button is released and software e-stop is not triggered. Wheels will resume acting upon software navigation commands. |

#### OperationState.Charging `enum`

| Name | Number | Description |
|------|--------|-------------|
| `CHARGING_UNKNOWN` | 0 | Default value. |
| `CHARGING_DISCHARGING` | 1 | No charging method is detected. The robot is able to navigate freely in this state. |
| `CHARGING_WIRED` | 2 | The robot is connected to a cable charger. Navigation-related commands will be ignored until the cable charger is unplugged. |
| `CHARGING_WIRELESS` | 3 | The robot is charging without being plugged into a cable charger. (i.e. inductive or contact charger) It is possible to navigate freely during this charging state. |

#### OperationState.Mission `enum`

| Name | Number | Description |
|------|--------|-------------|
| `MISSION_UNKNOWN` | 0 | Default value. |
| `MISSION_IDLE` | 1 | Indicates no mission is currently in progress irrespective of other states. |
| `MISSION_IN_PROGRESS` | 2 | The robot is currently on a mission. While in progress, the robot cannot accept any new mission. |

##### Example
=== "JSON"
    ```json
      {
        "system": "SYSTEM_OK",
        "systemMessage": "",
        "emergency": "EMERGENCY_DISENGAGED",
        "emergencyMessage": "",
        "charging": "CHARGING_DISCHARGING",
        "mission": "MISSION_IDLE"
      }
    ```
