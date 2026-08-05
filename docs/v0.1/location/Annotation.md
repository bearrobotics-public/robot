# Annotation

Annotations are used to define the areas on the map with specific parameters.

## Message Types

### Annotation
Annotations are used to define the areas on the map with specific parameters.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`annotation_id`   |`string`	| Example: "67305" |
|`created_time`	|[`Timestamp`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/timestamp.proto)	| The time when the annotation was created. |
|`display_name`	|`string`	| Example: "ITCT annotation A" |
|`obstacles`	|*repeated* [`Obstacle`](#obstacle)	| Areas on the map that the robot will try to avoid. |
|`parameter_zones`	|*repeated* [`ParameterZone`](Zones.md#parameterzone)	| Areas on the map that have specific parameters. |
|`destinations`	|*repeated* [`Destination`](#destination)	| Destinations are used to define the single point of interest. |
|`preferred_paths`	|*repeated* [`PreferredPath`](#preferredpath)	| Directional and Bi-Directional paths which robots will try to follow when nearby. |
|`queues`	|*repeated* [`Queue`](#queue)	| Queues are used to define the waiting area. |

##### Example
=== "JSON"
    ```json
      {
        "annotationId": "67305",
        "displayName": "ITCT annotation A",
        "obstacles": [],
        "parameterZones": [],
        "destinations": [],
        "preferredPaths": [],
        "queues": []
      }
    ```

### Destination
Destinations are used to define the single point of interest.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`destination_id`   |`string`	| The ID of the destination. |
|`display_name`	|`string`	| The display name of the destination. |
|`destination_pose`	|[`PointWithOrientation`](../common/Math.md#pointwithorientation)	| Position on the map where the robot would try to navigate to and orient itself along that direction. |
|`type`	|[`Type`](#destinationtype-enum) *enum*	| The type of the destination. |
|`docking_param`	|[`DockingParam`](#dockingparam)	| DockingParam is used to specify the docking process at the destination. If docking_param exists, docking is needed. If docking_param does not exist, no docking is needed. |
|`default_type_data`	|[`StringMapData`](#stringmapdata)	| Data specific to the destination type. The robot will use this data to interact with the destination. |

#### Destination.Type `enum`

| Name | Number | Description |
|------|--------|-------------|
| `TYPE_UNKNOWN` | 0 | Default value. |
| `TYPE_DEFAULT` | 1 | The default destination type. The robot will try to navigate to this point. |
| `TYPE_CONTACT_CHARGER` | 2 | The contact-type charger. The robot can charge through docking. |
| `TYPE_INDUCTIVE_CHARGER` | 3 | The inductive-type charger. The robot can charge through docking, but no physical contact is needed. |

##### Example
=== "JSON"
    ```json
      {
        "destinationId": "pickup_zone",
        "displayName": "Pickup Zone",
        "destinationPose": {
          "x": 2.5,
          "y": 3.0,
          "orientation": {
            "x": 0.0,
            "y": 0.0,
            "z": 0.0,
            "w": 1.0
          }
        },
        "type": "TYPE_DEFAULT"
      }
    ```

### DockingParam
DockingParam is used to specify the docking process at the destination.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`type`   |[`Type`](#dockingparamtype-enum) *enum*	| The type of docking process. |
|`reference`	|[`Reference`](#dockingparamreference-enum) *enum*	| The reference used to specify the docking position. |
|`reference_id`	|`string`	| The ID of the reference. |
|`tuning_params`	|[`Point2D`](../common/Math.md#point2d)	| The tuning parameters are used to define relative docking pose to the reference. |

#### DockingParam.Type `enum`

| Name | Number | Description |
|------|--------|-------------|
| `TYPE_UNKNOWN` | 0 | Default value. |
| `TYPE_DEFAULT` | 1 | The robot will run the default docking process at the destination. |

#### DockingParam.Reference `enum`

| Name | Number | Description |
|------|--------|-------------|
| `REFERENCE_UNKNOWN` | 0 | Default value. |
| `REFERENCE_DEFAULT` | 1 | The default reference is different for each destination type. For example, the default reference for TYPE_CONTACT_CHARGER is VL Marker. |
| `REFERENCE_QR_CODE` | 2 | The QR code reference is used to specify the docking position. |
| `REFERENCE_VL_MARKER` | 3 | The VL marker reference is used to specify the docking position. |

##### Example
=== "JSON"
    ```json
      {
        "type": "TYPE_DEFAULT",
        "reference": "REFERENCE_DEFAULT",
        "referenceId": "marker_001",
        "tuningParams": {
          "x": 0.0,
          "y": 0.0
        }
      }
    ```

### StringMapData
Data specific to the destination type.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`data`   |`map<string, string>`	| A map of key-value pairs. |

##### Example
=== "JSON"
    ```json
      {
        "data": {
          "key1": "value1",
          "key2": "value2"
        }
      }
    ```

### Obstacle
Areas on the map that the robot will try to avoid.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`obstacle_id`   |`string`	| The ID of the obstacle. |
|`points`	|*repeated* [`Point2D`](../common/Math.md#point2d)	| Points that define a polygon where the robot should avoid entering. The minimum number of points is 3. |
|`type`	|[`Type`](#obstacletype-enum) *enum*	| The type of the obstacle. |

#### Obstacle.Type `enum`

| Name | Number | Description |
|------|--------|-------------|
| `TYPE_UNKNOWN` | 0 | Default value. |
| `TYPE_SOFT_OBSTACLE` | 1 | Soft obstacle that the robot will try to avoid, but can still drive through if necessary. |
| `TYPE_RESTRICTED_OBSTACLE` | 2 | Restricted obstacle that the robot cannot enter and cannot exit if it becomes stuck inside this zone. |

##### Example
=== "JSON"
    ```json
      {
        "obstacleId": "obstacle_001",
        "points": [
          {"x": 0.0, "y": 0.0},
          {"x": 1.0, "y": 0.0},
          {"x": 1.0, "y": 1.0},
          {"x": 0.0, "y": 1.0}
        ],
        "type": "TYPE_SOFT_OBSTACLE"
      }
    ```

### PreferredPath
Directional and Bi-Directional paths which robots will try to follow when nearby.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`preferred_path_id`   |`string`	| The ID of the preferred path. |
|`graph_nodes`	|*repeated* [`GraphNode`](Types.md#graphnode)	| List of graph nodes that make up the preferred path. |

##### Example
=== "JSON"
    ```json
      {
        "preferredPathId": "path_001",
        "graphNodes": []
      }
    ```

### Queue
Queues are used to define the waiting area.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`queue_id`   |`string`	| The ID of the queue. |
|`queue_poses`	|*repeated* [`GraphNode`](Types.md#graphnode)	| Represents a list of queuing points where the robot will wait. |
|`destination_ids`	|`string` *repeated*	| These are end destinations that a queue can dequeue the robot to. |

##### Example
=== "JSON"
    ```json
      {
        "queueId": "queue_001",
        "queuePoses": [],
        "destinationIds": ["destination_001"]
      }
    ```
