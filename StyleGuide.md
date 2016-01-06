# 2016 Viking Robotics C++ Style Guide

##Description

This document details the coding styles that will be enforced throughout the year on all programming projects that pertain to Robotics. The rules are here to enforce a standard, and are meant to make things consistent so as to not confuse any future programmers. For any suggestions for changes to be made, either discuss it with the current head programmer(s) or, should they not be available, send them an email containing both what change you think should be made and why.

The current programming heads are:
Joseph Conlon Meek: `meekj@campbellhall.org`
Tyler Robbins:      `robbint@campbellhall.org`

##Naming Conventions

General
=======

Make sure names are short, but descriptive.
Do not abbreviate names unless there is a good reason to. Should you need to abbreviate a name, make sure you place a comment above its declaration stating the full, unabridged name. If abbreviations must be made, make sure that the variable can still be understood at a glance.

Examples:

```c++
// Good
CANTalon* frontLeft;
// Okay
float x;
// Bad
Solenoid* gabe;
ADXRS453Z* ggnore;
CANTalon* thisIsTheFrontLeftWheelOfTheRobotWhichUsesMecanumDriveAndHowCanWeBeRealIfOurEyesArentReal;
```

File Names
=======

File names should start with a capital letter and be in CamelCase with the first letter capitalized. Source files should use the `.cpp` extension and header files should use `.h`.

Examples:

```c++
AutonomousDrive.cpp // Camel case, ends in .cpp
AutonomousDrive.h // Camel case, ends in .h
ADXRS453Z.cpp // Name of a part.
PIDrift.cpp // Abbreviation of “Proportional-Integral-Derivative Drift Controller”
some_class.hpp // Bad. Not camel cased (snake cased instead), uses .hpp instead of .h
```

Type Names
=======

Type names should start with a capital letter and be camel cased. A type means either a class, struct, and enums.

Examples:

```c++
class Hardware{ … }
class AutonomousDrive{ … }
```


