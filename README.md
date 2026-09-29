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
- The SmartHomeController, who control the devices.
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

### Object Properties (16)

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
### Data Properties (7)

| Property | Domain | Range | Characteristics |
|---|---|---|---|
| `RoomName` | `Rooms` | `xsd:string` | **Functional** |
| `DeviceName` | `IoTDevices` | `xsd:string` | **Functional** |
| `DeviceStatus` | `IoTDevices` | `xsd:string` | **Functional** |
| `Measurementvalue` | `Sensors` | `xsd:decimal` | — |
| `ObservationValue` | `EnvironmentalObservation` | `xsd:4string` | — |
| `EventDescription` | `Events` | `xsd:string` | — |
| `EventTime` | `Events` | `xsd:dateTime` | — |

---

##  Individuals 

| Individual | Class | Notable assertions |
|---|---|---|
| `LivingRoom` | `Room` | Contains LRSmartDoor and LRLightSensor |
| `Kitchen` | `Room` | Contains KitchenSmokeSensor, KitchenTempSensor |
| `Bedroom` | `Room` | Contains BRMovementSensor, BRLockDoor |
| `BathRoom` | `Room` | - |
| `SmartCamera` | `CameraSensor` | locatedIn: LivingRoom |
| `LivingRoomLightObs` | `LightObserver` | locatedIn: LivingRoom |
| `BedRoomMotionSensor` | `MotionSensor` | locatedIn: BedRoom |
| `LivingRoomMotionSensor` | `MotionSensor` | locatedIn: LivingRoom |
| `MotionDetected` | `MovementDetected` | locatedIn: LivingRoom |
| `HomeSecurityController` | `SecurityController` | Controls: SmartLock, SmartCamera |
| `SecurityThreat01` | `SecurityThreat` | Controls: SmartLock |
| `MainHomeController` | `SmartHomeController` | Controls: SmartLock, SmartCamera |
| `BRSmartLock` | `SmartLock` | LocatedIn: BedRoom |
| `LRSmartLock` | `SmartLock` | LocatedIn: LivingRoom |
| `KitchenThermostat` | `SmartThermostat` | LocatedIn: Kitchen |
| `KitchenSmokeObs` | `SmartObserver` | LocatedIn: Kitchen |
| `TemperatureIncrease` | `TemperatureChange` | TemperatureValue: 24, OccursIn: Kitchen |

---
## Competency questions and DL queries
| Label | Competency Question | DL Query |
|---|---|---|
| `CQ1` | `Who is the resident at smarthome01` | smarthome:Resident and inverse smarthome:HasResident value smarthome:SmartHome01 |
| `CQ2` | `Which devices are the sensors` | smarthome:Sensors |
| `CQ3` | `Which IoTDevices are located in kitchen` | smarthome:IoTDevices and smarthome:LocatedIn value smarthome:Kitchen |
| `CQ4` | `Which controllers are controlling the actuators` | smarthome:Controllers and smarthome:Controls some smarthome:Actuators |
| `CQ5` | `Which events are occurring in living room` | smarthome:Events and smarthome:OccursIn value smarthome:LivingRoom |
| `CQ6` | `Which devices are controlled by controllers` | smarthome:IoTDevices and smarthome:ControlledBy some smarthome:Controllers |

---
## SWRL Rules
### Rule1: Smoke Location
If a smoke event is detected by smoke sensors, the event will occur in the room in which that sensor is presnt.

SmokeSensor(?s) ^
LocatedIn(?s, ?r) ^
Detects(?s, ?e) ^
SmokeDetected(?e)
→ OccursIn(?e, ?r)

### Rule2: Motion Location
This describes location of movement event from the place of the motion sensor.

MotionSensor(?s) ^
LocatedIn(?s, ?r) ^
Detects(?s, ?e) ^
MovementDetected(?e)
→ OccursIn(?e, ?r)

### Rule3: Motion Location
This rule determines about the temperature changes event that occurred. 

TemperatureSensor(?s) ^
LocatedIn(?s, ?r) ^
Detects(?s, ?e) ^
TemperatureChange(?e)
→ OccursIn(?e, ?r)

### Rule4: Smoke Safety trigger
A smoke event when occurred, can trigger the smart door to close located in the same room of event.

SmokeDetected(?e) ^
OccursIn(?e, ?r) ^
SmartDoor(?d) ^
LocatedIn(?d, ?r)
→ Triggers(?e, ?d)

### Rule5: SecurityAutomation
A security threat triggers the smart lock in the location of the security threat.

SecurityThreat(?e) ^
OccursIn(?e, ?r) ^
SmartLock(?l) ^
LocatedIn(?l, ?r)
→ Triggers(?e, ?l)

---

## How to Open

1. Download `smarthome.owl`
2. Open **Protégé 5.5**
3. File → Open → select `smarthome.owl`
4. Reasoner → **HermiT** → Start Reasoner
5. Open **SWRL** tab and run **Drools** to execute the rules.
6. Open **DLQuery** and enter competency questions and execute.
7. Use **OntoGraf** to visualize ontology relations
