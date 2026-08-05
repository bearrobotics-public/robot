# Robot

Robot messages provide information about the robot's pose, odometry, and physical state.

## Message Types

### Pose
Represents the robot pose.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`x_meters`   |`float`	| x, y coordinates inside the Map relative to the map's specified origin. |
|`y_meters`	|`float`	| y coordinate inside the Map relative to the map's specified origin. |
|`heading_radians`	|`float`	| The heading of the robot in radians. Ranges from -PI to PI, with 0.0 pointing along the x-axis. |

##### Example
=== "JSON"
    ```json
      {
        "xMeters": 2.5,
        "yMeters": 3.0,
        "headingRadians": 1.57
      }
    ```

### PoseWithCovariance
A pose and a covariance matrix.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`pose`   |[`Pose`](#pose)	| The robot pose. |
|`covariance`	|`double` *repeated*	| A 36-element array, which is a flattened 6×6 covariance matrix in row-major order. Each element represents the covariance between state variables (X, Y, Z, X-axis rotation, Y-axis rotation, Z-axis rotation). |

##### Example
=== "JSON"
    ```json
      {
        "pose": {
          "xMeters": 2.5,
          "yMeters": 3.0,
          "headingRadians": 1.57
        },
        "covariance": []
      }
    ```

### OdometryState
Represents the robot's odometry state.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`pose`   |[`Pose`](#pose)	| The current robot pose. |
|`twist`	|[`Twist`](../common/Math.md#twist)	| A message containing the linear and angular velocity of the robot. A positive linear velocity means the robot is moving forward in the direction of the Pose.heading, while a negative is opposite of the Pose.heading. A positive angular velocity means the robot is turning clockwise when looking from above, and negative is counter-clockwise when looking from above. If twist is 0, i.e. linear and angular velocities are 0, the robot is stationary. |

##### Example
=== "JSON"
    ```json
      {
        "pose": {
          "xMeters": 2.5,
          "yMeters": 3.0,
          "headingRadians": 1.57
        },
        "twist": {
          "linearVelocity": 0.5,
          "angularVelocity": 0.2
        }
      }
    ```
