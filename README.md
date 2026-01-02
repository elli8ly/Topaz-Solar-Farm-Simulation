# Topaz Solar Farm – pvlib Expected Production Model

This project implements a physics-based solar performance modeling workflow using **pvlib** to generate an expected AC production baseline for the Topaz Solar Farm. The model combines site-specific geographic information, real CEC-listed module and inverter specifications, electrical sizing logic, and PVGIS-derived weather data to produce a transparent and reproducible expected production benchmark.

---

## Project Objective

The objective of this project is to demonstrate how expected solar production can be modeled in a structured and defensible way when direct SCADA or revenue meter data is unavailable.

---

## Site Definition

The modeled system represents the Topaz Solar Farm using the following site parameters:

- **Latitude / Longitude**: 35.37495, -120.05370  
- **Time Zone**: US/Pacific  
- **Altitude**: 620 meters  
- **Tracking**: Fixed-tilt system  

The site location is used by pvlib to compute solar position, clear-sky irradiance, and time-aware modeling behavior.

---

## System Geometry and Configuration

- **Surface Tilt**: 25°  
- **Surface Azimuth**: 180° (south-facing)  

These assumptions represent a simplified fixed-tilt configuration suitable for expected production modeling and validation.

---

## Module and Inverter Selection

Electrical component parameters are pulled directly from the **California Energy Commission (CEC)** databases via pvlib:

- **Module**: `First_Solar__Inc__FS_390`  
- **Inverter**: `SMA_America__SC630CP_US__with_ABB_EcoDry_Ultra_transformer_`  

Using CEC-listed components ensures that all electrical and performance parameters are based on manufacturer datasheets rather than assumed values.

---

## Electrical Sizing Logic

### String Sizing

- Module open-circuit voltage (`Voc_ref`) and maximum power voltage (`Vmp_ref`) are extracted from the CEC module database.
- The inverter maximum DC voltage (`Vdcmax`) is used to constrain the number of modules per string.
- A conservative voltage margin of **95% of Vdcmax** is applied to prevent overvoltage conditions.

This results in a physically feasible number of modules connected in series per string.

---

### DC/AC Ratio and Equivalent Sizing

- **Target DC/AC Ratio**: 1.25  
- Module STC power is used to compute the DC nameplate capacity per string.
- The number of equivalent strings per inverter is calculated to meet the target DC/AC ratio.

To keep the pvlib system configuration manageable, a **representative block of 50 strings** is simulated, and all modeled AC outputs are scaled using a computed **scale factor** so results reflect the equivalent per-inverter DC sizing.

---

## Thermal and Loss Modeling

- **Temperature Model**: SAPM open-rack glass-glass  
- **Angle-of-Incidence (AOI) Model**: Physical  
- **Spectral Loss Model**: Disabled  

These selections represent common utility-scale assumptions and allow the model to focus on first-order physical behavior rather than detailed loss attribution.

---


## Limitations

- This model represents an **expected baseline**, not actual plant performance.  
- Electrical sizing checks are performed at reference (STC-like) conditions and do not include cold-temperature voltage corrections.  
- Detailed plant topology, availability losses, curtailment, degradation, and wiring losses are not modeled.  
- SCADA or revenue meter data is not included.

---

