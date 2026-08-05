# Map

Map messages provide information about maps and map content.

## Message Types

### Map
Represents a map within the robot's operational environment.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`map_id`   |`string`	| Example: "9578" |
|`created_time`	|[`Timestamp`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/timestamp.proto)	| The time when the map was created. |
|`modified_time`	|[`Timestamp`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/timestamp.proto)	| The time when the map was last modified. |
|`display_name`	|`string`	| Example: "ITCT SEOUL" |
|`map_data_id`	|`string`	| Current map data identifier that represents this map. |
|`annotation_ids`	|`string` *repeated*	| List of annotation identifiers associated with this map. |
|`current_annotation_id`	|`string`	| Current annotation identifier. |

##### Example
=== "JSON"
    ```json
      {
        "mapId": "9578",
        "displayName": "ITCT SEOUL",
        "mapDataId": "map_data_001",
        "annotationIds": ["67305"],
        "currentAnnotationId": "67305"
      }
    ```

### MapContent
Represents the content of a map which includes the map data and annotation.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`map_id`   |`string`	| The map identifier. |
|`data`	|[`Data`](#mapcontentdata)	| The map image data and metadata. |
|`annotation`	|[`Annotation`](Annotation.md#annotation)	| The annotation associated with this map. |

#### MapContent.Data
The image PNG data for the map.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`data`   |`bytes`	| The image PNG data for the map. |
|`origin`	|[`Origin`](#origin)	| The origin of the map. |
|`m_per_pixel`	|`float`	| Maps real-world size to pixelated size. (meters per pixel) This value is equivalent to the resolution defined in the ROS map server. https://wiki.ros.org/map_server |

#### MapContent.Annotation
The annotation associated with this map.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`destinations`   |*repeated* [`Destination`](Annotation.md#destination)	| List of destinations in the annotation. |

##### Example
=== "JSON"
    ```json
      {
        "mapId": "9578",
        "data": {
          "data": "<base64_encoded_png_data>",
          "origin": {
            "xM": 0.0,
            "yM": 0.0,
            "yawRadians": 0.0
          },
          "mPerPixel": 0.05
        },
        "annotation": {
          "destinations": []
        }
      }
    ```

### MapData
Represents the data of a map which includes the url, origin, and resolution.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`map_data_id`   |`string`	| The map data identifier. |
|`url`	|`string`	| URL to the image data for the map. |
|`origin`	|[`Origin`](#origin)	| The Origin of the map. |
|`m_per_pixel`	|`float`	| Maps real-world size to pixelated size (meters per pixel) This value is equivalent to the "resolution" defined in the ROS map server https://wiki.ros.org/map_server |

##### Example
=== "JSON"
    ```json
      {
        "mapDataId": "map_data_001",
        "url": "https://example.com/maps/map.png",
        "origin": {
          "xM": 0.0,
          "yM": 0.0,
          "yawRadians": 0.0
        },
        "mPerPixel": 0.05
      }
    ```

### Origin
The 2D pose of the map's origin (x, y, yaw) in meters and radians. This value is equivalent to the origin defined in the ROS map server. https://wiki.ros.org/map_server

| Field  | Message Type | Description |
|------------|-------------| ---|
|`x_m`   |`float`	| This is the (x, y) coordinate of the origin of the map in the world frame. |
|`y_m`	|`float`	| This is the y-coordinate of the origin of the map in the world frame. |
|`yaw_radians`	|`float`	| This is the rotation around the z-axis (counterclockwise) of the map with respect to the world frame. A yaw of 0 means no rotation. |

##### Example
=== "JSON"
    ```json
      {
        "xM": 0.0,
        "yM": 0.0,
        "yawRadians": 0.0
      }
    ```
