### Python Examples
This page provides example usage in Python with our APIs

#### Compiling Proto Files to Python
Use the `protoc` compiler to generate Python files from your `.proto` files.

For example, to drive the robot we need the Service from `robot_api_service.proto` and Twist message from `math.proto`
```bash
python -m grpc_tools.protoc -I=/path_to_repository/bearrobotics-public/robot/ --python_out=. --grpc_python_out=. bearrobotics/api/v0/robot/robot_api_service.proto

python -m grpc_tools.protoc -I=/path_to_repository/bearrobotics-public/robot/ --python_out=. bearrobotics/api/v0/common/math.proto 
```

#### Driving the Robot

**Prerequisite**: [Compiled](#compiling-proto-files-to-python) pb2 files for `robot_api_service` and `math`

**Script**: The following script turns the robot in place at 0.2 m/s at a set rate of 15Hz. To stop the robot from rotating, terminate the script.
```python
import time
import grpc
from math_pb2 import Twist
from robot_api_service_pb2 import DriveRobotRequest
from robot_api_service_pb2_grpc import RobotServiceStub

# Server details
SERVER_ADDRESS = "10.10.127.2:5123"

# Twist parameters
LINEAR_VELOCITY = 0.0  # m/s
ANGULAR_VELOCITY = 0.2  # rad/s
PUBLISH_RATE = 15  # Hz

def main():
    # Establish a gRPC channel
    channel = grpc.insecure_channel(SERVER_ADDRESS)
    stub = RobotServiceStub(channel)

    try:
        while True:
            # Create a Twist message
            twist = Twist(
                linear_velocity=LINEAR_VELOCITY,
                angular_velocity=ANGULAR_VELOCITY
            )

            # Create a DriveRobotRequest
            request = DriveRobotRequest(twist=twist)

            # Send the request 
            stub.DriveRobot(iter([request]))
            print(f"Published: linear_velocity={LINEAR_VELOCITY}, angular_velocity={ANGULAR_VELOCITY}")

            # Maintain 15 Hz publishing rate
            time.sleep(1 / PUBLISH_RATE)
    except KeyboardInterrupt:
        print("Stopped publishing.")

if __name__ == "__main__":
    main()
```
