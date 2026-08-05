# Settings

Settings messages allow configuration of robot settings.

## Message Types

### Setting
A single setting key-value pair.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`key`   |`string`	| The setting key. |
|`value`	|`string`	| The setting value. Both key and values are strings so types for each value must be filled as a string. |

##### Example
=== "JSON"
    ```json
      {
        "key": "robot-max-vel-x",
        "value": "0.8"
      }
    ```

### SettingsState
Represents the current settings state.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`settings`   |`map<string, string>`	| A map where each item is a pair of setting key and value. |

##### Example
=== "JSON"
    ```json
      {
        "settings": {
          "robot-max-vel-x": "0.8",
          "robot-enable-motors-coast-in-idle": "True"
        }
      }
    ```
