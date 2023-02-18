# Klipper Configuration

This is the Klipper config for my modded machine.

I have this cloned into a folder named `machine`, and my `printer.cfg looks` like:

```cfg
#Mainsail config
[include mainsail.cfg]


# System
[menu __main __octoprint]
type: disabled


# Other Config

[include machine/machine.cfg]

```


## Hardware

| Item            | Current                                                                 |
| --------------- | ----------------------------------------------------------------------- |
| Initial Printer | Ender 3 Pro
| Mainboard       | `Creality 4.2.2 C`[^1]
| Probe           | BLTouch 3.1
| Extruder        | `Revo Hemera-XS`
| Z Axis Mod      | Official Dual Z kit - went dual steppers for "PRUSA Style" alignment [^2]


[^1]: The C is written on the SD Card slot, and indicates the stepper driver type
[^2]: I planned to do a belt driven dual Z mod, but then I learned that you can "auto-level"
the x axis by carefully stalling the Z against end stops,
see [X-Axis / Dual Z Alignment](#x-axis--dual-z-alignment).


## X-Axis / Dual Z Alignment

> I'm going to convert this idea to Klipper eventually.

Example, description and GCode [in this reddit post by u/willsside](https://www.reddit.com/r/ender3v2/comments/oy0sct/calibrating_dual_z_steppers_prusa_style/).
Also explained in their [Mechanical Endstops for Z Axis Alignment](https://www.thingiverse.com/thing:4924870) project on [Thingiverse](https://www.thingiverse.com/thing:4924870).

I'm including their GCode below for posterity.

```gcode
; Align X Axis Gantry / Calibrate Dual Z Steppers

; Similar to M915: Mechanical Alignment

G28 ; Home all axes

G0 Z250 ; Go to Z Top Max

M211 S0 ; Disable Software Endstops

G91 ; Relative Positioning

G0 Z10 ; Move up 10 mm to push into mechanical endstops and align stepper steps

G0 Z-10 ; Move down 10 mm

G90 ; Absolute Positioning

M211 S1 ; Enable Software Endstops

G28 ; Home all axes

;G29 ; ABL - BLTouch

M18 S0 Z ; Prevents the idle disabling of the z-steppers

```
