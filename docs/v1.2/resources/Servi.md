# Servi

These endpoints and their message types are only available for the Servi robot family. Attempting to run a Servi command on a non-Servi robot will result in an `INVALID_ARGUMENT` error.

!!! note "Sending Servi missions"
    Servi missions and mission workflows are created through the shared mission endpoints. See [`CreateMission`](Mission.md#createmission) and [`CreateMissionWorkflow`](Mission.md#createmissionworkflow) on the Mission page for the `servi.Mission` and `servi.MissionWorkflow` payloads, and the `servi.Feedback` / `ServiType` values returned by [`SubscribeMissionStatus`](Mission.md#subscribemissionstatus).

------------

## CreateMission

Use the shared [`CreateMission`](Mission.md#createmission) / [`AppendMission`](Mission.md#appendmission) endpoints to send missions for Servi robots. Servi-specific missions are sent using the `servi_mission` payload below; preset workflows use the `servi_workflow` payload with [`CreateMissionWorkflow`](Mission.md#createmissionworkflow).

##### servi_mission `servi.Mission`
Use the field `servi_mission` to create and send a servi mission. The current API version supports 6 types of Servi mission.

| Field (*oneof*) | Message Type | Description |
|------------|-------------| ---|
|`delivery_mission`   |[`DeliveryMission`](#delivery_mission-deliverymission)	| Create a servi mission of type `Delivery`. |
|`bussing_mission`   |[`BussingMission`](#bussing_mission-bussingmission)	| Create a servi mission of type `Bussing`. |
|`delivery_patrol_mission`	|[`DeliveryPatrolMission`](#delivery_patrol_mission-deliverypatrolmission)| Create a servi mission of type `DeliveryPatrol`. |
|`bussing_patrol_mission`	|[`BussingPatrolMission`](#bussing_patrol_mission-bussingpatrolmission)| Create a servi mission of type `BussingPatrol`. |
|`navigate_mission`   |[`NavigateMission`](#navigate_mission-navigatemission)	| Create a servi mission of type `Navigate`. |
|`navigate_auto_mission`	|[`NavigateAutoMission`](#navigate_auto_mission-navigateautomission)| Create a servi mission of type `NavigateAuto`. |

#### delivery_mission `DeliveryMission`
A mission that navigates to one or more goals, stopping at each for a set amount of time or until some weight is removed.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| a list of `Goal` |
|`params`|[`DeliveryParams`](#deliveryparams-deliveryparams)| Parameters for delivery mission. |

#### bussing_mission `BussingMission`
A mission that navigates to one or more goals, stopping at each for a set amount of time or until some weight is added.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| a list of `Goal` |
|`params`|`BussingParams`|  ***There is no param defined in this API version.*** |

#### delivery_patrol_mission `DeliveryPatrolMission`
A mission that continuously loops through goals, stopping at each for a set amount of time or until all weight is removed.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| a list of `Goal` |
|`params`|`DeliveryPatrolParams`|  ***There is no param defined in this API version.*** |

#### bussing_patrol_mission `BussingPatrolMission`
A mission that continuously loops through goals, stopping at each for a set amount of time or until weight exceeds a threshold.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| a list of `Goal` |
|`params`|`BussingPatrolParams`|  ***There is no param defined in this API version.*** |

#### navigate_mission `NavigateMission`
A mission consisting of a single, explicitly defined goal.

| Field | Message Type | Description |
|------|------|-------------|
|`goal`| [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| Single target goal for navigation. |

#### navigate_auto_mission `NavigateAutoMission`
A mission that automatically selects the first unoccupied and unclaimed goal from the provided list, preferring goals with lower index values.

| Field | Message Type | Description |
|------|------|-------------|
|`goals`| *repeated* [`Goal`](LocalizationAndNavigation.md#goal) <br />`required`| List of potential goals to choose from. |

#### DeliveryParams `DeliveryParams`
Parameters for a delivery mission.

| Field | Message Type | Description |
|------|------|-------------|
|`tray_mappings`| *repeated* [`TrayMapping`](#traymapping-traymapping) | Tray mappings for the delivery mission. Only supported for Servi+ robots. |

#### TrayMapping `TrayMapping`
Mapping between a given tray to a goal. Note: Tray mapping is only supported for Servi+ robots.

| Field | Message Type | Description |
|------|------|-------------|
|`tray_name`| `string` | Name of the tray. |
|`goal`| [`Goal`](LocalizationAndNavigation.md#goal) | Target goal for this tray. |

##### JSON Request Example
=== "JSON"
    ```js
      {
        "serviMission": {
          "navigateMission": {
            "goal": {
              "pose": {
                "xMeters": 2.5,
                "yMeters": 1.8,
                "headingRadians": 1.57
              }
            }
          }
        }
      }
    ```

#### Servi workflow payloads (CreateMissionWorkflow) {#servi-workflow-payloads-createmissionworkflow}

!!! Note "CreateMissionWorkflow"
    When using [CreateMissionWorkflow](Mission.md#createmissionworkflow), set one of the following under `serviWorkflow`. Each type carries only goals, or a single goal for `birthday`, with no params.

    | Field (*oneof*) | Message Type | Description |
    |----------------|--------------|-------------|
    | `delivery` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |
    | `delivery_patrol` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |
    | `bussing` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |
    | `bussing_patrol` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |
    | `birthday` | [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | Single target goal. |
    | `hosting` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |
    | `hosting_patrol` | *repeated* [`Goal`](LocalizationAndNavigation.md#goal)<br/>`required` | a list of `Goal` |

##### JSON Request Example
=== "JSON"
    ```js
      {
        "missionWorkflow": {
          "serviWorkflow": {
            "delivery": {
              "goals": [
                { "destinationId": "table-1" },
                { "destinationId": "table-2" }
              ]
            }
          }
        }
      }
    ```

-----------

### TrayStatesWithMetadata
A snapshot of tray states reported by the robot, paired with event metadata.

| Field | Message Type | Description |
|------|------|-------------|
| `metadata` | [`EventMetadata`](Mission.md#eventmetadata) | Metadata associated with the tray states. |
| `tray_states` | [`TrayStates`](#traystates) | The tray states reported by the robot. |

#### TrayStates
A list of tray states reported by individual trays.

| Field | Message Type | Description |
|------|------|-------------|
| `tray_states` | *repeated* [`TrayState`](#traystate) | State of enabled trays, ordered from the top-most tray on the robot to the bottom. |

#### TrayState
Represents the state of a single tray.

| Field | Message Type | Description |
|------|------|-------------|
| `tray_name`  | `string` | Unique string name for the given tray. <br /> e.g. "top", "middle", "bottom". |
| `load_state` | [`LoadState`](#loadstate-enum) *enum* | Current load state of the tray. |
| `weight_kg` | `float` | Weight on the tray in kilograms. Minimum precision is 10g. |
| `load_ratio` | `float` | Ratio of the current load to the tray's maximum load capacity.<br />This value may exceed 1.0 if the tray is overloaded.<br /> Caveats:<br />- If the maximum load is misconfigured (e.g., set to 0.0), this value may return NaN. |

#### LoadState `enum`
| Name                   | Number | Description                                      |
|------------------------|--------|--------------------------------------------------|
| LOAD_STATE_UNKNOWN     | 0      | Default value. It means the `load_state` field is not returned. |
| LOAD_STATE_LOADED      | 1      | The tray has a valid load. |
| LOAD_STATE_EMPTY       | 2      | The tray is empty. |
| LOAD_STATE_OVERLOADED  | 3      | The tray is carrying more than its maximum capacity. |

##### Tray status example
=== "JSON"
    ```js
      {
        "metadata": {
          "timestamp": "2025-04-01T16:00:00Z",
          "sequenceNumber": 105
        },
        "trayStates": [
          {
            "trayName": "top",
            "loadState": "LOAD_STATE_OVERLOADED",
            "weightKg": 8.1,
            "loadRatio": 1.18
          },
          {
            "trayName": "middle",
            "loadState": "LOAD_STATE_LOADED",
            "weightKg": 2.3,
            "loadRatio": 0.76
          },
          {
            "trayName": "bottom",
            "loadState": "LOAD_STATE_EMPTY",
            "weightKg": 0,
            "loadRatio": 0
          }
        ]
      }
    ```

-----------

## CalibrateTrays
Calibrates the trays on the robot.

Only applicable for tray-equipped robots (e.g., Servi, Servi Plus).

Calibrates all trays if no tray names are provided.
Returns an `INVALID_ARGUMENT` error and rejects the request if any tray name is invalid.
Returns an empty response on success.

### Request

##### selector `servi.TraySelector` `required`
Selector to specify which trays to calibrate.

#### TraySelector
`servi.TraySelector` selects specific trays by name.

| Field | Message Type | Description |
|------|------|-------------|
| `tray_names` | *repeated* `string` | List of tray names to calibrate. If empty, calibrates all trays. |

##### JSON Request Example
=== "JSON"
    ```js
      {
        "selector": {
          "trayNames": ["top", "middle"]
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
| `INVALID_ARGUMENT` | This command is being sent to a non-Servi robot, or any tray name is invalid. |
| `INTERNAL` | Internal server error occurred while processing the request. |
