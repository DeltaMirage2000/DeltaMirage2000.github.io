---
layout: page
title: Flywheel log splitter
description: Preliminary design of a 90 kN kinetic log splitter, from functional analysis to gearbox sizing
importance: 3
category: /engineering
related_publications: false
---

- **Context:** Mechanical design project (COSI), Arts et Métiers, Feb–May 2024, team of 4
- **My role:** Geometric simulation of the mechanism, axle design, gearbox shafts and gears, splined fits, crank fixing, log-stop actuator, frame
- **Methods:** Functional analysis (FAST, SysML, octopus diagram), kinematics (GeoGebra), shaft and gear sizing

The client wanted an electric log splitter that splits logs up to 400 mm in diameter and 500 mm long, at 250 to 300 logs per hour, on a 2.2 kW three-phase motor. A 90 kN splitting force is far beyond what that motor can deliver directly, so we chose a **kinetic design**: the motor slowly spins up a flywheel, and the stored energy is released in one stroke through a crank, a vertical connecting rod and a toggle mechanism that drives the blade.

<figure style="text-align:center">
  <img src="/assets/img/projects/log_splitter_kinematics.jpg" alt="Kinematic diagram of the crank and toggle mechanism"
       style="width:100%; max-width:640px; border-radius:8px;">
  <figcaption>Crank and toggle mechanism: a 180 mm crank drives a 600 mm blade stroke.</figcaption>
</figure>

## Key numbers

- **Energy per stroke:** 8.1 kJ, stored in a flywheel sized for a 25% speed drop. The motor needs about 1.1 kW to recharge it between strokes, within the 2.2 kW budget.
- **Kinematics:** the blade hits the log at about 5.5 m/s and peaks at 6.8 m/s.
- **Loads:** a static analysis at impact gives 186 kN in the vertical rod and 137 kN in each toggle link.
- **Gearbox:** three stages (6 × 5 × 4 = 120:1), taking 192 N·m on the motor shaft to 23 kN·m at the output.

## Gearbox and mechanism design

- Built the geometric simulation of the crank and toggle mechanism, used to set the link lengths and check the blade stroke and speed.
- Designed the axles of the mechanism.
- Sized the shafts in torsion, and the 115 mm output shaft under combined bending and torsion (von Mises).
- Sized custom gears with the tooth-bending model. Output torques were too high for standard catalogue gears, so the last two stages use quenched 35NCD6 steel.
- Designed splined fits for the removable gears and the fixing of the output crank.

<figure style="text-align:center">
  <img src="/assets/img/projects/log_splitter_geogebra.png" alt="GeoGebra model of the crank and toggle mechanism"
       style="width:100%; max-width:450px; border-radius:8px;">
  <figcaption>GeoGebra model of the mechanism: the crank angle slider drives the toggle and the blade position.</figcaption>
</figure>

## Outcome

The final design meets the splitting force, cycle time and 30-year service life. It weighs **335 kg against a 250 kg target**: we chose strength over mass on the most loaded parts, and the solid gears could be lightened with cut-outs in a next iteration.

<figure style="text-align:center">
  <img src="/assets/img/projects/log_splitter_sketch.jpg" alt="Hand-drawn sketch of the complete log splitter"
       style="width:100%; max-width:420px; border-radius:8px;">
  <figcaption>Hand sketch of the full machine: motor, flywheel, gearbox, crank, toggle and slide.</figcaption>
</figure>

Team: Harold Gardon, Dorian Mandrino, Mateo Mangialomini, Alexandre Poncelet.
