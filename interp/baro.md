---
title: "Barometric interpolation"
author: "M. Marchildon"
date: '2021-09-03'
output: html_document
knit: (function(input_file, encoding) {
  out_dir <- '';
  rmarkdown::render(input_file,
  encoding=encoding,
  output_file=file.path(dirname(input_file), out_dir, 'baro.html'))})
---

## Summary

A methodology for interpolating hourly measured atmospheric pressure to a square-kilometre grid is described here. The initial intent for this interpolated product is to provide an estimate of atmospheric pressure at an arbitrary location for barometric corrections to groundwater monitors.

Even in the modest range of elevations found in the ORMGP jurisdiction (from Lake Ontario surface ~74 metres above sea level (masl) to just below 500 masl), atmospheric pressure can deviate up to 5 kPa.

This plot is based on the barometric formula with reference temperatures ($T_b$) from -20 to 30°C (shaded band):

$$
  P=P_b \cdot \left[\frac{T_b + L_b \cdot (h-h_b)}{T_b}\right]^{\frac{-g_0 \cdot M}{R^\cdot L_b}},
$$

| where:
|    $h_b$: reference elevation (masl)
|    $P_b$: reference pressure at elevation $h_b$ (Pa)
|    $P$: pressure at elevation $h$ (Pa)
|    $T_b$: reference temperature (K)
|    $L_0$ = -0.0065 K/m temperature lapse rate near earth's surface
|    $R^{*}$ = 8.3144598 J/(mol·K) universal gas constant
|    $g_{0}$ = 9.80665 m/s² gravitational acceleration
|    $M$ = 0.0289644 kg/mol molar mass of Earth's air
|    \
  
## Data Correction

Station data reporting hourly atmospheric pressures $P$ are first "corrected" to a reference elevation $h_b$, set here as the mean elevation of reporting weather stations at a given time step. With a reference temperature $T_b$ set to mean measured temperature, pressures are corrected to the reference elevation using the inverse of the barometric equation (note that the constants have been included):

$$
  P_b = P\cdot \left[1-0.0065 \frac{h-h_b}{T_b} \right]^{-5.26}.
$$

These "reference pressures" are then spatially interpolated to a grid using the nearest neighbour strategy and re-cast to their proper (DEM) elevation by:

$$
  P_z = P_b\cdot \left[1-0.0065 \frac{z-h_b}{T_b} \right]^{5.26},
$$

where $P_z$ is the pressure interpolated to DEM cell of elevation $z$.


## Results

Below is an animation of the hourly interpolation. Locations (points) and the surface are colour-coded the same; discrepancies are made evident where the colours mis-match.

<!-- ![GIF animation of final interpolation field](fig/baro.gif) -->
![GIF animation of final interpolation field points values and elevations are shown where data are available](https://raw.githubusercontent.com/OWRC/barometry/main/fig/baro.gif)
