# Robot API Service

The definition of Bear Robot API service.

### Map

#### GetLocation

- **Request Type:** [GetLocationRequest](#getlocationrequest)
- **Response Type:** [GetLocationResponse](#getlocationresponse)
- **Description:**
  Retrieve the current location data to which the robot is connected from
  the Universe. If the robot is offline, it uses the cached Location data.

#### GetMapContent

- **Request Type:** [GetMapContentRequest](#getmapcontentrequest)
- **Response Type:** [GetMapContentResponse](#getmapcontentresponse)
- **Description:**
  Retrieve the current map content data, which is loaded on the robot.

#### SwitchMap

- **Request Type:** [SwitchMapRequest](#switchmaprequest)
- **Response Type:** [SwitchMapResponse](#switchmapresponse)
- **Description:**
  Switches the current map to a specified map.<br>
  The request should specify a
  floor level and section index to be used. Returns the map_id of the switched
  map.

---

### Mission

#### AppendMission

- **Request Type:** [AppendMissionRequest](#appendmissionrequest)
- **Response Type:** [AppendMissionResponse](#appendmissionresponse)
- **Description:**
  Appends the given mission to the end of the queue.<br>
  The mission will be added in the order it is received.

#### ChargeRobot

- **Request Type:** [ChargeRobotRequest](#chargerobotrequest)
- **Response Type:** [ChargeRobotResponse](#chargerobotresponse)
- **Description:**
  Create a mission to go charge a robot regardless of battery state.<br>
  The call will fail if the robot is already on a different mission. The current
  mission needs to be canceled before the robot can be charged.

#### CreateMission

- **Request Type:** [CreateMissionRequest](#createmissionrequest)
- **Response Type:** [CreateMissionResponse](#createmissionresponse)
- **Description:**
  Creates a mission for a given type.<br>
  The call will fail if the robot cannot go on the requested mission.

#### SubscribeMissionStatus

- **Request Type:** [SubscribeMissionStatusRequest](#subscribemissionstatusrequest)
- **Response Type:** [SubscribeMissionStatusResponse](#subscribemissionstatusresponse)
- **Description:**
  Subscribe to robot's mission status.<br>
  Upon subscription, the server immediately sends the latest known mission
  status, followed by updates whenever the mission status changes.

#### UpdateMission

- **Request Type:** [UpdateMissionRequest](#updatemissionrequest)
- **Response Type:** [UpdateMissionResponse](#updatemissionresponse)
- **Description:**
  Updates the specified mission with the given command.<br>
  The call will fail if the robot is not on the specified mission or cannot
  execute the command.

---

### Navigation

#### DriveRobot

- **Request Type:** [DriveRobotRequest](#driverobotrequest)
- **Response Type:** [DriveRobotResponse](#driverobotresponse)
- **Description:**
  Manually drive the robot.<br>
  A fine grained level manual drive control that allows the user to specify a
  desired linear and angular velocity. The command will be smoothed by the
  robot. The request message should be streamed at a rate of at least 5Hz for
  smooth operation. If the frequency doesn't meet the requirements, it will set
  the commanded velocity to zero.

#### LocalizeRobot

- **Request Type:** [LocalizeRobotRequest](#localizerobotrequest)
- **Response Type:** [LocalizeRobotResponse](#localizerobotresponse)
- **Description:**
  Localize the robot to a localization goal.<br>
  If the goal is accepted, subcribe to SubscribeLocalizationStatus to get the
  localization status.

#### SetEmergencyStop

- **Request Type:** [SetEmergencyStopRequest](#setemergencystoprequest)
- **Response Type:** [SetEmergencyStopResponse](#setemergencystopresponse)
- **Description:**
  Enable or disable the emergency stop.

#### SetPose

- **Request Type:** [SetPoseRequest](#setposerequest)
- **Response Type:** [SetPoseResponse](#setposeresponse)
- **Description:**
  Manually set the robot pose given a pose on the map and covariance matrix.

#### SubscribeEmergencyStopStatus

- **Request Type:** [SubscribeEmergencyStopStatusRequest](#subscribeemergencystopstatusrequest)
- **Response Type:** [SubscribeEmergencyStopStatusResponse](#subscribeemergencystopstatusresponse)
- **Description:**
  Subscribe to the software emergency stop state.<br>
  Upon subscription, the server sends the current emergency stop state, followed
  by updates whenever the emergency stop state changes.

#### SubscribeLocalizationStatus

- **Request Type:** [SubscribeLocalizationStatusRequest](#subscribelocalizationstatusrequest)
- **Response Type:** [SubscribeLocalizationStatusResponse](#subscribelocalizationstatusresponse)
- **Description:**
  Subscribe to the robot's localization status.<br>
  Upon subscription, the latest known localization status will be sent. If the
  robot is actively localizing, status will be published upon changes.

#### SubscribeOdometryStatus

- **Request Type:** [SubscribeOdometryStatusRequest](#subscribeodometrystatusrequest)
- **Response Type:** [SubscribeOdometryStatusResponse](#subscribeodometrystatusresponse)
- **Description:**
  Subscribe to the robot's odometry status.<br>
  Upon subscription, the server provides regular updates (5Hz) of the odometry
  status.

#### SubscribeRobotPose

- **Request Type:** [SubscribeRobotPoseRequest](#subscriberobotposerequest)
- **Response Type:** [SubscribeRobotPoseResponse](#subscriberobotposeresponse)
- **Description:**
  Subscribe to the robot's pose.<br>
  Upon subscription, the server provides regular updates (5Hz) of the robot's
  estimated position.

---

### Settings

#### SetSetting

- **Request Type:** [SetSettingRequest](#setsettingrequest)
- **Response Type:** [SetSettingResponse](#setsettingresponse)
- **Description:**
  Set the specified setting.<br>
  The request will be rejected if the setting key does not exists.

#### SubscribeSettings

- **Request Type:** [SubscribeSettingsRequest](#subscribesettingsrequest)
- **Response Type:** [SubscribeSettingsResponse](#subscribesettingsresponse)
- **Description:**
  Upon subscription, the latest setting states will be sent.<br>
  For every setting change, a full snapshot of the states will be sent.

---

### Status

#### SubscribeBatteryStatus

- **Request Type:** [SubscribeBatteryStatusRequest](#subscribebatterystatusrequest)
- **Response Type:** [SubscribeBatteryStatusResponse](#subscribebatterystatusresponse)
- **Description:**
  Subscribe to the robot's battery status.<br>
  Upon subscription, the server immediately sends the latest known battery
  status, followed by updates whenever the battery status changes.

#### SubscribeOperationStatus

- **Request Type:** [SubscribeOperationStatusRequest](#subscribeoperationstatusrequest)
- **Response Type:** [SubscribeOperationStatusResponse](#subscribeoperationstatusresponse)
- **Description:**
  Subscribe to the robots's operation status.<br>
  Upon subscription, the server immediately sends the latest known operation
  status, followed by updates whenever the operation status changes.

---

### System

#### ConnectWifi

- **Request Type:** [ConnectWifiRequest](#connectwifirequest)
- **Response Type:** [ConnectWifiResponse](#connectwifiresponse)
- **Description:**
  Connect to a specified Wi-Fi network.<br>
  SSID should be provided and nearby broadcasted networks may be scanned with
  ListWifiConnections.

#### ForgetWifi

- **Request Type:** [ForgetWifiRequest](#forgetwifirequest)
- **Response Type:** [ForgetWifiResponse](#forgetwifiresponse)
- **Description:**
  Forget a saved Wi-Fi network.<br>
  When called, it forgets a Wi-Fi network identified by its ssid. The call will
  fail if a network operation is in progress or the network is not found.

#### GetSystemInfo

- **Request Type:** [GetSystemInfoRequest](#getsysteminforequest)
- **Response Type:** [GetSystemInfoResponse](#getsysteminforesponse)
- **Description:**
  Get the overall robot system information.<br>
  When called, the server returns robot system information. The system info
  tends to be static and does not change often.

#### ListWifiConnections

- **Request Type:** [ListWifiConnectionsRequest](#listwificonnectionsrequest)
- **Response Type:** [ListWifiConnectionsResponse](#listwificonnectionsresponse)
- **Description:**
  List available and remembered network connections.<br>
  When called, the server returns lists of remembered Wi-Fi networks, which may
  not necessarily be available, and returns lists of other available networks.

#### RunSystemCommand

- **Request Type:** [RunSystemCommandRequest](#runsystemcommandrequest)
- **Response Type:** [RunSystemCommandResponse](#runsystemcommandresponse)
- **Description:**
  Execute a system command on the robot.<br>
  Runs a system command. e.g. initiate robot reboot.
  Refer to the SystemCommand proto for all available commands.

#### SubscribeNetworkStatus

- **Request Type:** [SubscribeNetworkStatusRequest](#subscribenetworkstatusrequest)
- **Response Type:** [SubscribeNetworkStatusResponse](#subscribenetworkstatusresponse)
- **Description:**
  Subscribe to the robot's network status.<br>
  Upon subscription, the server immediately sends the latest known network
  status, followed by updates whenever the network status changes.

## Message Types

#### AppendMissionRequest

| Name      | Type                          | Description |
| --------- | ----------------------------- | ----------- |
| `mission` | [Mission](Mission.md#mission) | The mission to append to the queue. |

##### Request Example
=== "JSON"
    ```json
      {
        "mission": {
          "type": "TYPE_ONEOFF",
          "goals": [
            {
              "pose": {
                "xMeters": 2.5,
                "yMeters": 3.0,
                "headingRadians": 1.57
              }
            }
          ]
        }
      }
    ```

#### AppendMissionResponse

| Name         | Type   | Description                                    |
| ------------ | ------ | ---------------------------------------------- |
| `mission_id` | string | The unique identifier of the appended mission. |

##### Response Example
=== "JSON"
    ```json
      {
        "missionId": "cbd47ab1-df21-479e-9f72-677b81ab55b0"
      }
    ```

#### ChargeRobotRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### ChargeRobotResponse

| Name         | Type   | Description |
| ------------ | ------ | ----------- |
| `mission_id` | string | The ID of the mission created. |

##### Response Example
=== "JSON"
    ```json
      {
        "missionId": "mission-xyz-001"
      }
    ```

#### ConnectWifiRequest

| Name                 | Type                                              | Description                                                                                |
| -------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `ssid`               | string                                            | SSID of Wi-Fi network.                                                                     |
| `authentication`     | [Authentication](Network.md#authentication)       | Security details for the network.<br>This field can be omitted if the network is unsecure. |
| `connection_options` | [ConnectionOptions](Network.md#connectionoptions) | Optional parameters for static IP configuration.                                           |

##### Request Example
=== "JSON"
    ```json
      {
        "ssid": "MyWiFiNetwork",
        "authentication": {
          "password": "mypassword"
        }
      }
    ```

#### ConnectWifiResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### CreateMissionRequest

| Name      | Type                          | Description |
| --------- | ----------------------------- | ----------- |
| `mission` | [Mission](Mission.md#mission) | The mission to create. |

##### Request Example
=== "JSON"
    ```json
      {
        "mission": {
          "type": "TYPE_ONEOFF",
          "goals": [
            {
              "pose": {
                "xMeters": 2.5,
                "yMeters": 3.0,
                "headingRadians": 1.57
              }
            }
          ]
        }
      }
    ```

#### CreateMissionResponse

| Name         | Type   | Description |
| ------------ | ------ | ----------- |
| `mission_id` | string | The ID of the mission created. |

##### Response Example
=== "JSON"
    ```json
      {
        "missionId": "cbd47ab1-df21-479e-9f72-677b81ab55b0"
      }
    ```

#### DriveRobotRequest

| Name    | Type                             | Description                                            |
| ------- | -------------------------------- | ------------------------------------------------------ |
| `twist` | [Twist](../common/Math.md#twist) | The desired max linear and angular velocity to travel. |

##### Request Example
=== "JSON"
    ```json
      {
        "twist": {
          "linearVelocity": 0.5,
          "angularVelocity": 0.2
        }
      }
    ```

#### DriveRobotResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### ForgetWifiRequest

| Name   | Type   | Description |
| ------ | ------ | ----------- |
| `ssid` | string | SSID of the Wi-Fi network to forget. |

##### Request Example
=== "JSON"
    ```json
      {
        "ssid": "MyWiFiNetwork"
      }
    ```

#### ForgetWifiResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### GetLocationRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### GetLocationResponse

| Name       | Type                                         | Description |
| ---------- | -------------------------------------------- | ----------- |
| `location` | [Location](../location/Location.md#location) | The current location data to which the robot is connected. |

##### Response Example
=== "JSON"
    ```json
      {
        "location": {
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
      }
    ```

#### GetMapContentRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### GetMapContentResponse

| Name          | Type                                        | Description |
| ------------- | ------------------------------------------- | ----------- |
| `map_content` | [MapContent](../location/Map.md#mapcontent) | The current map content data loaded on the robot. |

##### Response Example
=== "JSON"
    ```json
      {
        "mapContent": {
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
      }
    ```

#### GetSystemInfoRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### GetSystemInfoResponse

| Name          | Type                               | Description |
| ------------- | ---------------------------------- | ----------- |
| `system_info` | [SystemInfo](System.md#systeminfo) | The robot system information. |

##### Response Example
=== "JSON"
    ```json
      {
        "systemInfo": {
          "softwareVersion": "servi-24.03",
          "firmwareVersion": "3.2.4.1",
          "robotFamily": "ROBOT_FAMILY_SERVI",
          "robotId": "pennybot-abc123",
          "displayName": "Sir V",
          "localeLanguage": "en-US",
          "wifiInfo": {
            "currentSsid": "MyWiFiNetwork",
            "cidrIp": "192.168.1.123/24",
            "gatewayIp": "192.168.1.1",
            "macAddress": "aa:1a:a1:a1:1a:11",
            "dnsIps": ["8.8.8.8", "8.8.4.4"]
          }
        }
      }
    ```

#### ListWifiConnectionsRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### ListWifiConnectionsResponse

| Name               | Type                                          | Description |
| ------------------ | --------------------------------------------- | ----------- |
| `wifi_connections` | [WifiConnections](Network.md#wificonnections) | Lists of remembered and available Wi-Fi networks. |

##### Response Example
=== "JSON"
    ```json
      {
        "wifiConnections": {
          "savedNetworks": [
            {
              "ssid": "MyWiFiNetwork",
              "signalStrength": 85,
              "security": "SECURITY_PASSWORD_SECURED",
              "connectedState": "CONNECTION_BEAR_CONNECTED"
            }
          ],
          "availableNetworks": [
            {
              "ssid": "OtherNetwork",
              "signalStrength": 60,
              "security": "SECURITY_UNSECURED",
              "connectedState": "CONNECTION_UNKNOWN"
            }
          ]
        }
      }
    ```

#### LocalizeRobotRequest

| Name   | Type                                                 | Description |
| ------ | ---------------------------------------------------- | ----------- |
| `goal` | [LocalizationGoal](Localization.md#localizationgoal) | The localization goal. The robot must be placed within a 5x5 meter window from the localization goal. |

##### Request Example
=== "JSON"
    ```json
      {
        "goal": {
          "pose": {
            "xMeters": 2.5,
            "yMeters": 3.0,
            "headingRadians": 1.57
          }
        }
      }
    ```

#### LocalizeRobotResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### RunSystemCommandRequest

| Name             | Type                                     | Description |
| ---------------- | ---------------------------------------- | ----------- |
| `system_command` | [SystemCommand](System.md#systemcommand) | The system command to execute. |

##### Request Example
=== "JSON"
    ```json
      {
        "systemCommand": {
          "reboot": {}
        }
      }
    ```

#### RunSystemCommandResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### SetEmergencyStopRequest

| Name           | Type                                               | Description |
| -------------- | -------------------------------------------------- | ----------- |
| `e_stop_state` | [EmergencyStopState](Status.md#emergencystopstate) | The emergency stop state to set. |

##### Request Example
=== "JSON"
    ```json
      {
        "eStopState": {
          "emergency": "EMERGENCY_ENGAGED"
        }
      }
    ```

#### SetEmergencyStopResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### SetPoseRequest

| Name                   | Type                                              | Description                                                                                                |
| ---------------------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `pose_with_covariance` | [PoseWithCovariance](Robot.md#posewithcovariance) | A pose and a covariance matrix, if the covariance is not set, the internal default values will be applied. |

##### Request Example
=== "JSON"
    ```json
      {
        "poseWithCovariance": {
          "pose": {
            "xMeters": 2.5,
            "yMeters": 3.0,
            "headingRadians": 1.57
          },
          "covariance": []
        }
      }
    ```

#### SetPoseResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### SetSettingRequest

| Name      | Type                           | Description |
| --------- | ------------------------------ | ----------- |
| `setting` | [Setting](Settings.md#setting) | The setting to set. |

##### Request Example
=== "JSON"
    ```json
      {
        "setting": {
          "key": "robot-max-vel-x",
          "value": "0.8"
        }
      }
    ```

#### SetSettingResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeBatteryStatusRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeBatteryStatusResponse

| Name            | Type                                                    | Description |
| --------------- | ------------------------------------------------------- | ----------- |
| `metadata`      | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `battery_state` | [BatteryState](Status.md#batterystate)                  | The current battery state. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "batteryState": {
          "chargePercent": 85,
          "state": "STATE_CHARGING",
          "chargeMethod": "CHARGE_METHOD_WIRELESS"
        }
      }
    ```

#### SubscribeEmergencyStopStatusRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeEmergencyStopStatusResponse

| Name           | Type                                                    | Description |
| -------------- | ------------------------------------------------------- | ----------- |
| `metadata`     | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `e_stop_state` | [EmergencyStopState](Status.md#emergencystopstate)      | The current emergency stop state. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "eStopState": {
          "emergency": "EMERGENCY_DISENGAGED"
        }
      }
    ```

#### SubscribeLocalizationStatusRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeLocalizationStatusResponse

| Name                 | Type                                                     | Description |
| -------------------- | -------------------------------------------------------- | ----------- |
| `metadata`           | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `localization_state` | [LocalizationState](Localization.md#localizationstate)   | The current localization state. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "localizationState": {
          "state": "STATE_LOCALIZING"
        }
      }
    ```

#### SubscribeMissionStatusRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeMissionStatusResponse

| Name            | Type                                                    | Description |
| --------------- | ------------------------------------------------------- | ----------- |
| `metadata`      | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `mission_state` | [MissionState](Mission.md#missionstate)                 | The current mission state of the robot. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "missionState": {
          "missionId": "d6637a14-5f6b-43f6-bd86-cc1871a8322e",
          "state": "STATE_RUNNING",
          "goals": [
            {
              "pose": {
                "xMeters": 4.2,
                "yMeters": 7.8,
                "headingRadians": 1.57
              }
            }
          ],
          "currentGoalIndex": 1,
          "navigationStatus": "NAVIGATION_STATUS_NAVIGATING"
        }
      }
    ```

#### SubscribeNetworkStatusRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeNetworkStatusResponse

| Name            | Type                                                    | Description |
| --------------- | ------------------------------------------------------- | ----------- |
| `metadata`      | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `network_state` | [NetworkState](Network.md#networkstate)                 | The current network state. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "networkState": {
          "connectedWifi": {
            "ssid": "MyWiFiNetwork",
            "signalStrength": 85,
            "security": "SECURITY_PASSWORD_SECURED",
            "connectedState": "CONNECTION_BEAR_CONNECTED"
          }
        }
      }
    ```

#### SubscribeOdometryStatusRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeOdometryStatusResponse

| Name             | Type                                                    | Description |
| ---------------- | ------------------------------------------------------- | ----------- |
| `metadata`       | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `odometry_state` | [OdometryState](Robot.md#odometrystate)                 | The current odometry state. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "odometryState": {
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
      }
    ```

#### SubscribeOperationStatusRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeOperationStatusResponse

| Name              | Type                                                    | Description |
| ----------------- | ------------------------------------------------------- | ----------- |
| `metadata`        | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `operation_state` | [OperationState](Status.md#operationstate)              | The current operation state. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "operationState": {
          "system": "SYSTEM_OK",
          "systemMessage": "",
          "emergency": "EMERGENCY_DISENGAGED",
          "emergencyMessage": "",
          "charging": "CHARGING_DISCHARGING",
          "mission": "MISSION_IDLE"
        }
      }
    ```

#### SubscribeRobotPoseRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeRobotPoseResponse

| Name       | Type                                                    | Description |
| ---------- | ------------------------------------------------------- | ----------- |
| `metadata` | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `pose`     | [Pose](Robot.md#pose)                                   | The current robot pose. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "pose": {
          "xMeters": 2.5,
          "yMeters": 3.0,
          "headingRadians": 1.57
        }
      }
    ```

#### SubscribeSettingsRequest

- _(No fields defined)_

##### Request Example
=== "JSON"
    ```json
      {}
    ```

#### SubscribeSettingsResponse

| Name             | Type                                                    | Description |
| ---------------- | ------------------------------------------------------- | ----------- |
| `metadata`       | [EventMetadata](../common/Annotations.md#eventmetadata) | Event metadata including timestamp and sequence number. |
| `settings_state` | [SettingsState](Settings.md#settingsstate)              | The current settings state. |

##### Response Example
=== "JSON"
    ```json
      {
        "metadata": {
          "timestamp": "2025-04-01T15:30:00Z",
          "sequenceNumber": 128
        },
        "settingsState": {
          "settings": {
            "robot-max-vel-x": "0.8",
            "robot-enable-motors-coast-in-idle": "True"
          }
        }
      }
    ```

#### SwitchMapRequest

| Name            | Type  | Description |
| --------------- | ----- | ----------- |
| `floor_level`   | int32 | The floor level to switch to. |
| `section_index` | int32 | The section index to switch to. |

##### Request Example
=== "JSON"
    ```json
      {
        "floorLevel": 0,
        "sectionIndex": 0
      }
    ```

#### SwitchMapResponse

| Name     | Type   | Description |
| -------- | ------ | ----------- |
| `map_id` | string | The ID of the switched map. |

##### Response Example
=== "JSON"
    ```json
      {
        "mapId": "9578"
      }
    ```

#### UpdateMissionRequest

| Name              | Type                                        | Description |
| ----------------- | ------------------------------------------- | ----------- |
| `mission_command` | [MissionCommand](Mission.md#missioncommand) | Command to update the state of an active mission. |

##### Request Example
=== "JSON"
    ```json
      {
        "missionCommand": {
          "missionId": "f842c8ac-62de-412e-90fb-bf37022db2f4",
          "command": "COMMAND_PAUSE"
        }
      }
    ```

#### UpdateMissionResponse

- _(No fields defined)_

##### Response Example
=== "JSON"
    ```json
      {}
    ```
