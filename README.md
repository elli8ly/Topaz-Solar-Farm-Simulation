# Topaz Solar Farm – Expected Production Modeling (pvlib)

This project builds an expected AC production baseline for the Topaz Solar Farm using the open-source **pvlib** library. The model combines site-specific location and geometry, real CEC-listed module and inverter specifications, and weather data from **PVGIS** to estimate how the plant should perform under typical meteorological conditions.

The system is modeled as a fixed-tilt, south-facing array using a representative electrical configuration sized to a target DC/AC ratio of 1.25. Module string lengths are constrained by inverter DC voltage limits, and a representative block of strings is simulated and scaled to reflect equivalent per-inverter capacity. These assumptions are intended to capture first-order system behavior rather than detailed plant topology.

Weather inputs are sourced from the **PVGIS Typical Meteorological Year (TMY)** dataset, which represents a long-term, weather-normalized “typical” year constructed from historical data. The TMY data includes irradiance (GHI, DNI, DHI), temperature, and wind speed, and is indexed as a single 8760-hour year for consistency and ease of analysis. This allows expected production to be estimated without sensitivity to any specific historical year.

Before applying the TMY weather, a clear-sky simulation is run as a validation step to confirm that the modeled system produces physically reasonable irradiance and power profiles. Once validated, the model is driven by TMY weather to generate hourly AC power and monthly expected energy production. These results provide a stable expected baseline that can later be compared against measured SCADA or revenue meter data.

This project is intended as a transparent, reproducible example of how expected solar production can be modeled when measured operational data is unavailable. It is not a design-grade simulation and does not include availability losses, curtailment, degradation, or detailed electrical topology, but it reflects common assumptions used in utility-scale solar performance analysis.
