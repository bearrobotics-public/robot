# Robot System

System-level operations for an individual robot.

-----------
## RunSystemCommand

Execute a OS-level command on a robot.

!!! Note
    When rebooting the robot, a response will return immediately to acknowledge the request but may take several minutes before the robot reconnects.

### Request

##### system_command `SystemCommand` `required`
The system command to execute on the robot.

##### SystemCommand
| Field (*oneof*) | Message Type | Description |
|------------|-------------| ---|
| `reboot` | [`Reboot`](#reboot) | Reboot the robot with specified type. |
| `shutdown` | [`Shutdown`](#shutdown) | Shut down the robot system. |

#### Reboot
| Field | Message Type | Description |
|------|------|-------------|
| `type` | [`Type`](#type-enum) *enum* | The type of reboot to perform. |

#### Type `enum`
| Name | Number | Description |
|------|--------|-------------|
| TYPE_UNKNOWN | 0 | Default value. This should never be used explicitly. |
| TYPE_SOFTWARE_ONLY | 1 | Perform an OS-level reboot without powering off hardware devices. |
| TYPE_WITH_HARDWARE | 2 | Perform a full power-cycle including hardware devices. |

#### Shutdown
*(No fields defined)*

##### JSON Request Example
=== "Reboot"
    ```js
      {
        "systemCommand": {
          "reboot": {
            "type": "TYPE_SOFTWARE_ONLY"
          }
        }
      }
    ```
=== "Shutdown"
    ```js
      {
        "systemCommand": {
          "shutdown": {}
        }
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
| `INVALID_ARGUMENT` | Invalid request: `system_command` missing, reboot with `type: TYPE_UNKNOWN`, or unknown command type. |
| `INTERNAL` | The request failed to execute due to internal error. Client should retry the command. |
