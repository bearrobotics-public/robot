# Locations & Maps

[`Destination`](#destination) describes a point of interest defined in the robot's map; a mission [`Goal`](LocalizationAndNavigation.md#goal) refers to one by its `destination_id`.

------------

### Destination
A single point of interest on the map that a robot can navigate to and align itself with.

| Field | Type | Description |
|-------|------|-------------|
| `destination_id` | `string` | Unique identifier for the destination. |
| `display_name` | `string` | Human-readable name for the destination. |
| `pose` | [`Pose`](LocalizationAndNavigation.md#pose) | Position of the destination in the robot's map coordinate system. |

##### JSON Example
=== "JSON"
    ```js
      {
        "destinationId": "table_12",
        "displayName": "Table 12",
        "pose": {
          "xMeters": 4.2,
          "yMeters": 7.8,
          "headingRadians": 1.57
        }
      }
    ```
