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
Examples
========
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
