# Types

Types messages provide common types used in location-related messages.

## Message Types

### GraphNode
Represents a node in a graph structure used for navigation.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`graph_node_id`   |`string`	| The ID of the graph node. |
|`graph_node_pose`	|[`PointWithOrientation`](../common/Math.md#pointwithorientation)	| Point with orientation of the GraphNode. |
|`adjacent_graph_node_ids`	|`string` *repeated*	| Adjacent GraphNode IDs that robot can navigate from the current GraphNode. |

##### Example
=== "JSON"
    ```json
      {
        "graphNodeId": "node_001",
        "graphNodePose": {
          "x": 2.5,
          "y": 3.0,
          "orientation": {
            "x": 0.0,
            "y": 0.0,
            "z": 0.0,
            "w": 1.0
          }
        },
        "adjacentGraphNodeIds": ["node_002", "node_003"]
      }
    ```
