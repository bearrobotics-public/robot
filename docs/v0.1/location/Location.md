# Location

Location messages provide information about the location to which the robot is connected.

## Message Types

### Location
Represents a specific location within the robot's operational environment.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`location_id`   |`string`	| Example: "4RVF" |
|`created_time`	|[`Timestamp`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/timestamp.proto)	| The time when the location was created. |
|`modified_time`	|[`Timestamp`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/timestamp.proto)	| The time when the location was last modified. |
|`display_name`	|`string`	| Examples: "City Deli & Grill", "KNTH" |
|`floors`	|`map<int32, Floor>`	| Map of floor identifiers to floor information. The key is the floor level, and the value is the floor information. |

#### Location.Floor
Represents a floor within a location.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`display_name`   |`string`	| Example: "Ground" |
|`sections`	|*repeated* [`Section`](#locationfloorsection)	| List of sections on this floor. |

##### Location.Floor.Section
Represents a section of a floor. Each Section corresponds to an area on a floor that may have its own map(s). We assume sections are disconnected; if connected sections are needed in the future, additional information will be added to represent the connections.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`display_name`   |`string`	| Usually display_name will be empty if the section is not named. |
|`map_ids`	|`string` *repeated*	| List of map identifiers associated with this section. |
|`current_map_id`	|`string`	| Current map identifier. |

##### Example
=== "JSON"
    ```json
      {
        "locationId": "4RVF",
        "displayName": "City Deli & Grill",
        "floors": {
          "0": {
            "displayName": "Ground",
            "sections": [
              {
                "displayName": "Main Floor",
                "mapIds": ["9578"],
                "currentMapId": "9578"
              }
            ]
          }
        }
      }
    ```
