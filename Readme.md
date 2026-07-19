# OrcaSlicer Profiles for BCN3D Sigma D25 (WIP)

This repository contains configuration files (printer, filament, and process profiles) to use the **BCN3D Sigma D25** IDEX 3D printer with **OrcaSlicer**.

Since OrcaSlicer does not (yet) include a native profile for BCN3D printers, this project is a community effort to get the printer up and running.

**Status:** Work in Progress (WIP) – Basic single-extruder prints are working, but the profile still needs work to achieve full IDEX functionality. Contributions (Pull Requests) are highly encouraged!

---

## 🎯 The Goal

The goal is to create a fully functional profile for the Sigma D25 in OrcaSlicer that supports:

* Correct mechanical dimensions and homing routines.
* Utilization of both extruders (IDEX Toolchange).
* Compatibility with the BCN3D touch display (SD card printing).

## 🚀 What already works

* **Base profile:** The printer is set up as a "Custom Marlin" with the correct dimensions (420x300x200 mm) and the appropriate *Gantry Height* (12 mm).
* **Filament:** The profile is adapted to the BCN3D standard of **2.85 mm** filament diameter (crucial, as Orca defaults to 1.75 mm!).
* **Start and End G-Code (Basic):** The standard heating, homing, and parking routines have been translated from BCN3D Stratos to OrcaSlicer.
* **Cloud printing:** Single prints utilizing the left extruder (T0) can be successfully started when the `.gcode` file is uploaded via the BCN3D Cloud.

## ⚠️ Known Issues & What DOES NOT work yet (To-Do's)

Community help is needed here:

* ❌ **SD card printing is blocked:** The printer's touch display (frontend) blocks `.gcode` files from the SD card. The BCN3D OS looks for very specific header information and Cura formats that OrcaSlicer does not natively generate. *(Workaround: Uploading via the BCN3D Cloud works, as this bypasses the strict frontend).*
* ❌ **Print time missing on display:** The display shows `0h 0m` during printing because it cannot find the Cura-specific timestamp (e.g., `;TIME:3723`) in the G-code.
* ❌ **Right extruder & Toolchange untested:** The G-code for the tool change from T0 to T1 is currently missing. Dual-color prints or switching to support material have not been tested yet.
* ❌ **Purge Buckets are not used:** OrcaSlicer cannot parse the proprietary BCN3D macros (like `{purge_in_bucket_before_start_gcode}`) and throws slicing errors. These had to be removed from the start code. *(Workaround: Currently, skirts/brims must be used to prime the nozzle before printing).*
* ❌ **IDEX Special Modes:** Hardware modes like "Duplication" or "Mirror" are currently not supported.

## 💡 Our "Learnings" & Technical Details so far

If you want to tinker with the G-code, keep these things in mind:

1. **Orca Parser vs. BCN3D Macros:** OrcaSlicer strictly tries to parse everything inside curly brackets `{...}` as a variable. Even with a semicolon `;` in front of it (commenting it out), this causes a parser error for unknown BCN3D macros. Variables must either be translated into OrcaSlicer's square brackets (e.g., `[first_layer_temperature]`) or deleted completely.
2. **Dynamic Acceleration:** The commands `M204` and `M205` (Acceleration & Jerk) were removed from the start/end code because OrcaSlicer excellently calculates and controls these values dynamically on its own.
3. **Z-Offsets & Extruder Offsets:** The offsets for X, Y, and Z intentionally remain at `0` in the slicer. The printer handles the calibration of the two printheads relative to each other internally via its own firmware and touch display.

## 🛠 How to use these profiles

1. Download the `.json` files from this repository.
2. Open OrcaSlicer.
3. Go to **File -> Import -> Import Configurations** and select the downloaded files.
4. When slicing, currently always add **3-4 Skirt lines** to manually prime the nozzle (since the purge bucket code is missing).
5. Upload the exported G-code file via the **BCN3D Cloud** to start the print.
