# Math

Math messages provide common mathematical types used throughout the API.

## Message Types

### Point2D
Represents a 2D point in space with x and y coordinates.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`x`   |`float`	| The x coordinate of the point in a 2D plane. |
|`y`	|`float`	| The y coordinate of the point in a 2D plane. |

##### Example
=== "JSON"
    ```json
      {
        "x": 2.5,
        "y": 3.0
      }
    ```

### Quaternion
Represents a rotation in 3D space using four components (x, y, z, w).

| Field  | Message Type | Description |
|------------|-------------| ---|
|`x`   |`float`	| The x component of the quaternion (imaginary part). |
|`y`	|`float`	| The y component of the quaternion (imaginary part). |
|`z`	|`float`	| The z component of the quaternion (imaginary part). |
|`w`	|`float`	| The real (scalar) component of the quaternion. |

##### Example
=== "JSON"
    ```json
      {
        "x": 0.0,
        "y": 0.0,
        "z": 0.0,
        "w": 1.0
      }
    ```

### PointWithOrientation
Defines a 2D point (x, y) with a 3D orientation.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`x`   |`float`	| The x coordinate of the point. |
|`y`	|`float`	| The y coordinate of the point. |
|`orientation`	|[`Quaternion`](#quaternion)	| The orientation represented as a quaternion. |

##### Example
=== "JSON"
    ```json
      {
        "x": 2.5,
        "y": 3.0,
        "orientation": {
          "x": 0.0,
          "y": 0.0,
          "z": 0.0,
          "w": 1.0
        }
      }
    ```

### Twist
This expresses velocity in 2D space in linear and angular components.

| Field  | Message Type | Description |
|------------|-------------| ---|
|`linear_velocity`   |`float`	| The desired speed to drive up to in meters-per-second (m/s). A positive value should be used for driving forward and a negative value may be used to drive in reverse. |
|`angular_velocity`	|`float`	| The desired rate of rotation in radians-per-second (rad/s). A positive value is used for a clockwise (right) point turn while a negative value should be used for counter-clockwise (left) point turn. |

##### Example
=== "JSON"
    ```json
      {
        "linearVelocity": 0.5,
        "angularVelocity": 0.2
      }
    ```
