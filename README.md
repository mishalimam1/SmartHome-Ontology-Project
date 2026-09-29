# Smart-Home-Ontology-Project
### Knowledge Representation — University of Verona
**Author: ** Mishal Imam

## Overview of the Project

The **Smart-Home Ontology** is an ontology developed using **OWL 2** to model the knowledge domain of a Smart-Home system. It provides a structured representation of Smart-Home devices, Smart-Home areas, Controllers, Sensors, actuators, IoTdevices, IoTServices and other events included in a Smart-Home system.

The ontology is designed for use with ontology engineering tools such as **Protégé** and supports semantic reasoning through OWL 2 reasoners like **HermiT** and **Pellet**.

---
## Objectives of the Project

The main objectives of this ontology project are:

- Represent a home which includes rooms where smart devices can be installed.
- Installing IoTdevices like sensors, actuators and controllers.
- The SmartHomeController who control the devices.
- Sensors installed in desired rooms and events detected.
- Connecting everything together.

---
## Ontology Structure

### Classes
The ontology contains **37 classes and subclasses** shown below, 

#### Primitive Classes (19)

| Branch | Classes |
|---|---|
| **SmartHome** | `SmartHome`, `Rooms` |
| **IoTDevices** | `Sensors`, `Actuators`, `Controllers` |
| **Controllers** | `SmartController`, `SecurityController` |
| **Actuators** | `SmartLight`, `Thermostat`, `SmartLock`, `SmartDoor` |
| **Sensors** | `TemperatureSensor`, `MotionSensor`, `SmokeSensor`, `CameraSensor` |
| **Event** | `MovementDetected`, `SmokeDetected`, `SecurityThreat`, `TemperatureChange` |
| **Person** | `Resident`, `Guest` |
| **EnvironmentalObservation** | `TemperatureObserver`, `LightObserver`, `SmokeObserver` |

---
### Product Hierarchy

```
owl:Thing
│
├── SmartHome
│
├── Rooms
│
├── Person
│   ├── Resident
│   └── Guest
│
├── IoTDevices
│   ├── Sensors
│   │   ├── TemperatureSensor
│   │   ├── MotionSensor
│   │   └── SmokeSensor
│   │   └── CameraSensor
│   │
│   ├── Controllers
│   │   ├── SmartHomeController
│   │   └── SecurityController
│   │
│   └── Actuators
│       ├── SmartLight
│       ├── SmartThermostat
│       ├── SmartLock
│       └── SmartDoor
│
├── Events
│   ├── TemperatureChange
│   ├── MovementDetected
│   ├── SmokeDetected
│   └── SecurityThreat
│
└── EnvironmentalObservation
    ├── TemperatureObserver
    ├── LightObserver
    └── SmokeObserver

```

## Data Properties

The ontology uses data properties to store information.

It includes:

- `DeviceName`
- `DeviceStatus`
- `EventDescription`
- `EventTime`
- `MeasurementValue`
- `ObservationTime`
- `ObservationValue`
- `PersonName`
- `RoomName`

---

### Object Properties (13)

| Property | Domain | Range | Characteristics |
|---|---|---|---|
| `ContainsDevice` | `Rooms` | `IoTDevices` | **Functional**, Inverse: `LocatedIn` |
| `LocatedIn` | `IoTDevices` | `Rooms` | Inverse: `ContainsDevice` |
| `ControlledBy` | `IoTDevices` | `Controllers` | Inverse: `Controls` |
| `Controls` | `Controllers` | `IoTDevices` | Inverse: `ControlledBy` |
| `Detects` | `Sensors` | `Events` | — |
| `HasDevice` | `SmartHome` | `IoTDevices` | — |
| `HasSensor` | `Rooms` | `Sensors` | — |
| `HasController` | `SmartHome` | `Controllers` | — |
| `HasGuest` | `SmartHome` | `Guest` | — |
| `Detects` | `Sensors` | `Event` | — |
| `HasResident` | `SmartHome` | `Resident` | — |
| `HasRooms` | `SmartHome` | `Rooms` | Inverse: `IsRoomOf` |
| `IsRoomOf` | `Rooms` | `SmartHome` | Inverse: `HasRoom` |
| `Observes` | `Sensors` | `EnvironmentalObservation` | — |
| `OccursIn` | `Events` | `Rooms` | — |
| `Triggers` | `Events` | `Actuators` | — |

---
### Data Properties (9)

| Property | Domain | Range | Characteristics |
|---|---|---|---|
| `HasBatteryLevel` | `IoTDevices` | `xsd:decimal` | **Functional** |
| `HasDeviceID` | `IoTDevices` | `xsd:string` | **Functional** |
| `HasHumidityValue` | `HumidityObservation` | `xsd:decimal` | **Functional** |
| `HasIlluminationValue` | `IlluminationObserver` | `xsd:decimal` | — |
| `HasIPAddress` | `IoTDevices` | `xsd:4string` | — |
| `HasPowerConsumption` | `IoTDevicesr` | `xsd:string` | — |
| `HastemperatureValue` | `TemperatureObservation` | `xsd:decimal` | — |
| `HasServiceName` | `IoTDevices` | `xsd:string` | — |
| `isActive` | `IoTDevices` | `xsd:boolean` | — |

---

## ABox — Individuals (20)

| Individual | Class | Notable assertions |
|---|---|---|
| `LivingRoom` | `Room` | Contains HueBulb_01, EntranceCam, FrontDoorLock |
| `Kitchen` | `Room` | Contains Nest_Thermo, KitchenTempSensor |
| `Bedroom` | `Room` | Contains BedroomLight, BedroomMotion |
| `SecurityRoom` | `Room` | Contains only SecurityMotion (Sensor) |
| `Garden` | `OutdoorSpace` | Monitored by GardenCam |
| `LivingArea` | `Zone` | hasRoom: LivingRoom, Kitchen |
| `NightZone` | `Zone` | hasRoom: Bedroom |
| `GardenCam` | `Camera` | monitors: Garden |
| `EntranceCam` | `Camera` | monitors: LivingRoom, locatedIn: LivingRoom |
| `BedroomMotion` | `MotionSensor` | locatedIn: Bedroom |
| `SecurityMotion` | `MotionSensor` | locatedIn: SecurityRoom |
| `KitchenTempSensor` | `TemperatureSensor` | temperatureValue: 21.5 |
| `Nest_Thermo` | `Thermostat` | locatedIn: Kitchen |
| `HueBulb_01` | `Light` | isActive: true, brightnessLevel: 80 |
| `BedroomLight` | `Light` | isActive: **false**, brightnessLevel: 0 |
| `FrontDoorLock` | `SmartLock` | controlled by MainHub AND OwnerPhone |
| `MainHub` | `HomeHub` | controls: HueBulb_01, Nest_Thermo, BedroomLight, FrontDoorLock |
| `OwnerPhone` | `MobileApp` | controls: FrontDoorLock, GardenCam |
| `NightMode` | `Scene` | activated by MainHub and OwnerPhone |
| `HomeWifi` | `Network` | All devices connected here |

---

## How to Open

1. Download `smarthome.owl`
2. Open **Protégé 5.5**
3. File → Open → select `smarthome.owl`
4. Reasoner → **HermiT** → Start Reasoner
