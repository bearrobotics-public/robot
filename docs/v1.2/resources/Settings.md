# Settings

Read, write, and reset the robot's key-value configuration.

-----------
## GetSettings

Get the specified settings.

!!! Note
    Response order is not guaranteed to match the request order.

### Request

##### keys `repeated string` `required`
The keys of the settings to retrieve.

##### JSON Request Example
=== "JSON"
    ```js
      {
        "keys": [
          "navigation.max_linear_speed",
          "sound.volume"
        ]
      }
    ```

### Response

##### settings `repeated Setting`
The requested settings with their current values.

##### Setting
A key-value pair for robot configuration.

| Field | Message Type | Description |
|------|------|-------------|
| `key` | `string` | The setting key. |
| `value` | `string` | The setting value, encoded as a string. |

##### JSON Response Example
=== "JSON"
    ```js
      {
        "settings": [
          {
            "key": "navigation.max_linear_speed",
            "value": "0.8"
          },
          {
            "key": "sound.volume",
            "value": "60"
          }
        ]
      }
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INVALID_ARGUMENT` | `keys` is empty. |
| `NOT_FOUND` | One or more of the given keys does not exist. |
| `INTERNAL` | Internal server error occurred while processing the request. |

-----------
## GetAllSettings

Get a snapshot of all settings.

### Request

*(No fields defined)*

##### JSON Request Example
=== "JSON"
    ```js
      {}
    ```

### Response

##### settings `repeated Setting`
A snapshot of all settings with their current values. See [`Setting`](#setting).

##### JSON Response Example
=== "JSON"
    ```js
      {
        "settings": [
          {
            "key": "navigation.max_linear_speed",
            "value": "0.8"
          },
          {
            "key": "sound.volume",
            "value": "60"
          }
        ]
      }
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INTERNAL` | Internal server error occurred while processing the request. |

-----------
## SetSetting

Set the specified setting.

!!! Note
    Rejected if the setting key does not exist.

### Request

##### setting `Setting` `required`
The setting key and value to set. See [`Setting`](#setting).

##### JSON Request Example
=== "JSON"
    ```js
      {
        "setting": {
          "key": "sound.volume",
          "value": "75"
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
| `INVALID_ARGUMENT` | The setting key does not exist, or the value is invalid for the key. |
| `INTERNAL` | Internal server error occurred while processing the request. |

-----------
## ResetSettings

Reset the specified settings to their default values.

### Request

##### keys `repeated string` `required`
The keys to reset to their default values.

##### unrecognized_key_policy `UnrecognizedKeyPolicy` `enum`
How unrecognized keys are handled.

##### UnrecognizedKeyPolicy `enum`
| Name | Number | Description |
|------|--------|-------------|
| UNRECOGNIZED_KEY_POLICY_UNKNOWN | 0 | Unspecified. Server applies default behavior (currently: REJECT). |
| UNRECOGNIZED_KEY_POLICY_SKIP | 1 | Skip unrecognized keys and continue; skipped keys are reported in the response. |
| UNRECOGNIZED_KEY_POLICY_REJECT | 2 | Reject the entire RPC if any key is unrecognized. |

##### JSON Request Example
=== "JSON"
    ```js
      {
        "keys": [
          "navigation.max_linear_speed",
          "sound.volume"
        ],
        "unrecognizedKeyPolicy": "UNRECOGNIZED_KEY_POLICY_SKIP"
      }
    ```

### Response

##### unrecognized_keys `repeated string`
Unrecognized keys from the request. Populated only when `unrecognized_key_policy` is `UNRECOGNIZED_KEY_POLICY_SKIP`.

##### JSON Response Example
=== "JSON"
    ```js
      {
        "unrecognizedKeys": [
          "sound.volume"
        ]
      }
    ```

### Errors
| ErrorCode  | Description |
|------------|-------------|
| `INVALID_ARGUMENT` | `keys` is empty. |
| `NOT_FOUND` | One or more keys were not recognized (when `unrecognized_key_policy` is `UNRECOGNIZED_KEY_POLICY_REJECT` or `UNRECOGNIZED_KEY_POLICY_UNKNOWN`). |
| `INTERNAL` | Internal server error occurred while processing the request. |
