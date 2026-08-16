# Design Document


## 1 System Specifications

The system is a small-batch fluid-bed coffee roaster designed to roast up to 500 g of green coffee using electrically heated air. The roaster uses a vacuum blower to force air through a heating element and into the bottom of the roast chamber. The airflow provides both the heat required for roasting and the force required to circulate the coffee beans.

The design is intended to operate from a 120 VAC, 20 A electrical circuit to allow operation without requiring a 240 V supply. The system will incorporate variable heater output and variable blower speed so that heat input and bean circulation can be controlled independently throughout the roast.

An ESP32-based control system will collect temperature and process data and control the heater and blower. Initial roast monitoring and profile visualization will be performed using Artisan. A custom user interface may be developed in a later revision.


### 1.1 Design Targets

| Parameter | Initial Target |
|---|---|
| Maximum batch size | 500 g |
| Nominal batch size | 350–400 g |
| Electrical supply | 120 VAC / 20 A |
| Heater capacity | ~1.7–1.8 kW |
| Roast method | Fluid-bed / spouted-bed |
| Roast chamber diameter | ~100 mm ID |
| Primary nozzle diameter | 35 mm |
| Test nozzle diameters | 30 / 35 / 40 mm |
| Target airflow | ~25–45 CFM |
| Blower type | Centrifugal |
| Main controller | ESP32 |
| Roast software | Vacuum |
| Temperature sensors | K-type thermocouples |

### 1.2 Constraints

The user may enter in invalid values for job attributes for weight settings and the app must be able to handle those errors. The design must remain platform-independent and should not include Android-specific classes such as Activities or Fragments. Weight values for job attributes must be between 0-9 (inclusive). where 0 means 'don't care' and default values are set to 1. A user may only have one current job.

### 1.3 System Environment

The application is designed as a standalone client application running on a mobile device. The application will utilize SQLite database for information persistence. The system executes entirely on the user's device (android os) using local storage. 

## 2 Mechanical Design

The architecture provides the high-level design view of a system and provides a basis for more detailed design work. These subsections describe the top-level components of the system you are building and their relationships.


### 2.1 Component Diagram

The application follows a simple object-oriented architecture consisting of four primary classes and the SQL database:


## 3 Electrical Design

The architecture provides the high-level design view of a system and provides a basis for more detailed design work. These subsections describe the top-level components of the system you are building and their relationships.


### 2.1 Component Diagram

The application follows a simple object-oriented architecture consisting of four primary classes and the SQL database:

![Component Diagram](./images/component-diagram.png)

## 4 Software/Controls Design

The architecture provides the high-level design view of a system and provides a basis for more detailed design work. These subsections describe the top-level components of the system you are building and their relationships.


### 2.1 Component Diagram

The application follows a simple object-oriented architecture consisting of four primary classes and the SQL database:

