# System

System messages provide information about the robot's system configuration and allow execution of system commands.

## Message Types

### SystemCommand
System operation to be executed. An empty message for the specified command should be provided. Future commands may provide arguments specific to the command.

| Field (*oneof*) | Message Type | Description |
|------------|-------------| ---|
|`reboot`   |[`RebootCommand`](#systemcommandrebootcommand)	| Provide an empty RebootCommand message to initiate a reboot. |

#### SystemCommand.RebootCommand
An empty message used to initiate a robot reboot.

*(No fields defined)*

##### Example
=== "JSON"
    ```json
      {
        "reboot": {}
      }
    ```

### SystemInfo
Provides information related to the system.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`software_version`   |`string`	| Distribution version. e.g. `"servi-24.03"` |
|`firmware_version`	|`string`	| Firmware version. e.g. `"3.2.4.1"` |
|`robot_family`	|[`RobotFamily`](#systeminforobotfamily-enum) *enum*	| Robot product family. |
|`robot_id`	|`string`	| Unique robot ID. e.g. `"pennybot-abc123"` |
|`display_name`	|`string`	| Display name set for the robot. e.g. `"Sir V"` |
|`locale_language`	|`string`	| IETF language tag represented in string. e.g. `"en-US"` |
|`wifi_info`	|[`WifiInfo`](Network.md#wifiinfo)	| Various information of the wireless interface and connected Wi-Fi network. |

#### SystemInfo.RobotFamily `enum`

| Name | Number | Description |
|------|--------|-------------|
| `ROBOT_FAMILY_UNKNOWN` | 0 | Default value. |
| `ROBOT_FAMILY_SERVI` | 1 | Servi robot family. |
| `ROBOT_FAMILY_SERVI_MINI` | 2 | Servi Mini robot family. |
| `ROBOT_FAMILY_SERVI_PLUS` | 3 | Servi Plus robot family. |
| `ROBOT_FAMILY_SERVI_LIFT` | 4 | Servi Lift robot family. |

##### Example
=== "JSON"
    ```json
      {
        "softwareVersion": "servi-24.03",
        "firmwareVersion": "3.2.4.1",
        "robotFamily": "ROBOT_FAMILY_SERVI",
        "robotId": "pennybot-abc123",
        "displayName": "Sir V",
        "localeLanguage": "en-US",
        "wifiInfo": {
          "currentSsid": "MyWiFiNetwork",
          "cidrIp": "192.168.1.123/24",
          "gatewayIp": "192.168.1.1",
          "macAddress": "aa:1a:a1:a1:1a:11",
          "dnsIps": ["8.8.8.8", "8.8.4.4"]
        }
      }
    ```

### SystemState
Represents the system health state.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`health`   |[`Health`](#systemstatehealth-enum) *enum*	| State representing whether robot is in a healthy condition. |

#### SystemState.Health `enum`

| Name | Number | Description |
|------|--------|-------------|
| `HEALTH_UNKNOWN` | 0 | Default value. |
| `HEALTH_OK` | 1 | Robot is in a healthy condition. |
| `HEALTH_ERROR` | 2 | Robot has health issues. |

##### Example
=== "JSON"
    ```json
      {
        "health": "HEALTH_OK"
      }
    ```
