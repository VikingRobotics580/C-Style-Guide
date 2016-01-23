# 2016 Viking Robotics C++ Style Guide

##Description

This document details the coding styles that will be enforced throughout the year on all programming projects that pertain to Robotics. The rules are here to enforce a standard, and are meant to make things consistent so as to not confuse any future programmers. For any suggestions for changes to be made, either discuss it with the current head programmer(s) or, should they not be available, send them an email containing both what change you think should be made and why.

In order to link to an anchor, make sure you are viewing this in [Github through Web Browser](https://github.com/VikingRobotics580/C-Style-Guide/blob/master/StyleGuide.md), hover over a header, click the link icon next to it, and copy the URL. If you know what an anchor is and plan to use one, you probably didn't need that thorough explanation.

The current programming heads are:
Joseph Conlon Meek: `meekj@campbellhall.org`
Tyler Robbins:      `robbint@campbellhall.org`

##Naming Conventions

###General


Make sure names are short, but descriptive.
Do not abbreviate names unless there is a good reason to. Should you need to abbreviate a name, make sure you place a comment above its declaration stating the full, unabridged name. If abbreviations must be made, make sure that the variable can still be understood at a glance.

Examples:

```c++
// Good
CANTalon* frontLeft;
// Okay
float x;
Robert roberts;
// Bad
Solenoid* gabe;
ADXRS453Z* ggnore;
CANTalon* thisIsTheFrontLeftWheelOfTheRobotWhichUsesMecanumDriveAndHowCanWeBeRealIfOurEyesArentReal;
```

###File Names

File names should start with a capital letter and be in CamelCase with the first letter capitalized. Source files should use the `.cpp` extension and header files should use `.h`.

Examples:

```c++
AutonomousDrive.cpp // Camel case, ends in .cpp
AutonomousDrive.h // Camel case, ends in .h
ADXRS453Z.cpp // Name of a part.
PIDrift.cpp // Abbreviation of “Proportional-Integral-Derivative Drift Controller”
some_class.hpp // Bad. Not camel cased (snake cased instead), uses .hpp instead of .h
```

###Type Names

Type names should start with a capital letter and be camelCased. A type means either a class, struct, and enums.

Examples:

```c++
class Hardware{ … }
class AutonomousDrive{ … }
```

###Variable Names

* Common Names
    * Variable names should start with a lower case letter and be camelCased.
    * Example: `int thingCounter;`
* Class Data Members
    * Class data members should be just like common names (camelCase), only with a leading m_.
    * Example: `int m_screenWidth;`
* Struct Data Members
    * Struct data members should be named just like common names (camelCase).
    * Example: `struct personData { ... };`
* Global Variables
    * Global variables should be in snake cased and preceded by a g_.
    * Example: `int g_screen_width;`
* Constant Variables
    * Constants should be in ALL CAPS and snake_cased.
    * This takes precedence over global variable formatting - use all caps and snake case for any constants.
    * Example: `const float SALES_TAX = 0.925;`
* Namespaces
    * Should be all lower case and snake cased.
* Enums
    * Should be named like constants.
    * Example: `enum TORTURE_TYPES`

###Function Names

* Regular Functions
    * Functions should start with a lower case letter and camel cased after that
    * Example: `int destroyHumans() { ... }`
* Accessors and Mutators (aka Getters and Setters)
    * Should be named the same as regular functions and should
start with the word 'get' or 'set' depending on what it is going to do and the member name in question
    * Examples: `void setSolenoidValue(bool value) { ... }`
    * `bool getSolenoidValue() { ... }`

##Header Files

### General

Every source file should have an associated header file. There are exceptions to this, such as in files containing just a main() function.

###Self-contained Headers

All header files should be self-contained. This means that header files should not perform any excecution or defining of their own. They should only perform declarations and should be wrapped in include guards.

Exceptions to this include template and inline functions, where the definition must not be seperated from its declaration.

###Include Guards

All header files should have an include guard to prevent multiple inclusion. The format should be *\_\<PATH\>\_\<TO\>\_\<FILE\>\_H\_* in all caps.

They must be like this to ensure the uniqueness of the include guards.

For example, if you have a file _src/auto/AutonomousDrive.h_, then your include guard should be as follows:

```c++
#ifndef _SRC_AUTO_AUTONOMOUSDRIVE_H_
#define _SRC_AUTO_AUTONOMOUSDRIVE_H_

// Your code here

#endif
```

###Forward Declarations

A forward declaration is the declaration of a class, function, or template without an associated definition

Avoid forward declarations using unless you are only refering to the object in question, or if you need to make sure something exists before it is included for some reason.

The reasons for this are that forward declarations, while it can both speed up compile time by reducing the files read and recompiled, have some problems associated with them.

For example, in this situation, a forward declaration is good:

```c++
// B.h
#ifndef _SRC_FOO_H_
#define _SRC_FOO_H_

class A;

class B{
 public:
  A some_method();
};
#endif
```

This is a good use of forward declarations because it just uses them to show that `B::some_method` is going to return an A object, thus there is not reason to `#include "A.h"` in this file.

However, if you are going to do anything more than reference the class, then you should simple `#include` the file.

How to decide:
 * Don't use forward declarations on types from another project (WPILib, STL, Boost, etc...)
 * When doing anything more than refering to a type, always `#include` the header file.
 * When using a class template, `#include` its header file.

Remember, if you can't tell whether you need to use a forward declaration or a `#include`, use a `#include`.

###Inline Functions

An inline function is a way of telling the compiler to expand functions inline rather than calling them through the usual mechanism.

Declare a function as inline only when it is very short (5 lines or less).

Example:

```c++
class A{
 public:
   inline bool getValue(){
      return m_value;
   };
   inline void setValue(bool val){
      m_value = val;
   };
 private:
   bool m_value;
};
```

Inlining a function could make much more efficient code, due to the fact that they place the body directly where they are called, rather than jumping to where the function is defined. However, if inlining is overused, the code could become less efficient because if a very large function is inlined, then the final program's size could increase dramatically, making execution much more difficult for the processor.

How to decide:
 * Inline a function if it is 5 lines of code or less
 * Do not inline destructors (destructors usually call other destructors, which can start to mount up after a while)
 * Avoid inlining a function if it has loops or swich statements unless you are sure that it will increase performance

Important Note: Even if you declare a function inline, sometimes the compiler will decide to not inline the function if it decides that a significant performance drop will occur.

###Order of Includes

When including a file, always refer to it from the project's base directory, avoiding Unix shortcuts like `.` and `..` whenever possible.

For example, including `RobotCode2016/src/foo/myAwesomeCode.h` should be done as:

```c++
#include "src/foo/myAwesomeCode.h"
```

Files should always be included in the following order:

1. The current file's associated header file (if one exists)
2. Any C system files
3. Any C++ system files
4. Any other libraries you may be using
5. Your project's header files

For example, let's assume we have the following directory structure:

 - src/
  - main.cpp
  - foo/
   - B.h
   - B.cpp
  - bar/
   - A.h
   - A.cpp

If we want `B.cpp` to include `A.h`, the top of it's code should look like the following:

```c++
// The associated header file.
#include "foo/B.h"

// C system file
#include <sys/types.h>

// C++ system file
#include <vector>

// Another library that we are using
#include "WPILib.h"

// Another file in our project.
#include "bar/A.h"
```

An exception to this ordering rule is if system-specific code needs conditional includes. This conditional include should be placed after the rest of the includes (if you need multiple conditional includes, then place them in the order mentioned above).

```c++
#include "foo/B.h"

#include "bar/A.h"

#ifdef LANG_CXX11
#include <initializer_list>
#endif
```

##Scoping

###Namespaces

Unless you are defining a class, place everything into a namespace. Namespaces should all have a unique name based on the data they encapsulate. Do not _ever_ use the `using` directive. Do not ever declare a namespace as `inline`. 

A namespace is a way to divide the global scope up into seperate named scopes. This helps prevent name collisions.

For example, if two files have a class named `A`, the two symbols may collide at either compile-time or run-time. If both files have their class in a namespace, `file1::A` and `file2::A`, both classes are distinct symbols and can both be used in the same file without worry about collisions.

Inlining a namespace will automatically place their names in the enclosing scope. For example:
```c++
namespace X {
 inline namespace Y {
  void foo();
 }
}
```

Now `X::Y::foo()` and `X::foo()` are equivalent. Inlining namespaces are primarily used for [ABI](https://en.wikipedia.org/wiki/Application_binary_interface) compatibility.

Try not to nest namespaces too deeply, as this could cause the code to become cluttered and confusing.

We do not use the `using` directive because it completely defeats the purpose of having a namespace in the first place.

###Non-member, Static, and Global Functions & Variables

Always prefer to wrap non-member or non-local variables into a namespace rather than making them global. Always prefer to wrap non-member functions in a namespace rather than making them global or making them static members of a class.

##Classes

###Constructors

Do not do any work in constructors beyond basic initialization. If additional initialization needs to be done (as in, calling a method or function), then that should be placed in a specialized Init method.

Bad:

```c++

class X {
  public:
    X(){
     a = 5;
     this->start();
    }
    void start(){ /* ... */ }
   private:
    int a;
};
```

Good:

```c++
class X {
 public:
  X(){
   a=5;
  }
  void start(){ /* ... */ }
 private:
  void Init(){
   this->start();
  }
  int a;
};
```

###Implicit Conversions

Implicit conversions allow an object of one type to be used where an object of a different type is expected, such as passing an int to a function that expects a double parameter.

Users can define their own conversions in addition to the ones provided by the language through the use of constructors. For example, suppose a class `X` and a function `f`:

```c++
class X {
 public:
  X(int a);
};

void f(X obj){ /* ... */ }
```

Now suppose the following code:

```c++
f(5);
```

This will _not_ result in an error because C++ provides for the implicit conversion of one type to another so long as a constructor or conversion function of the destination type exists.

However, C++ also provides the `explicit` keyword. This keyword forces the person calling the function to explicitly state the type being passed.

So, if we were to modify our example from earlier:

```c++
class X {
 public:
  explicit X(int a);
};
```

Now, calling `f(5);` will result in an error. This is preferable because it makes the code clearer to somebody reading if they don't need to determine that an implicit conversion is happening.

Do not use `explicit` for copy and move constructors (since no conversion takes place) or for constructors that take a single `std::initializer_list` parameter to support copy-initialization (Example: `X x = {1,2};`).
