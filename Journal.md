### Journal 1, 30/09/26

#### Task 1. Rogue game legend:

☺: Represents the player
The player character is shown in #FFFF55

A-Z: Represents enemies the player must fight
Shown in #AAAAAA

║, ═, ╚, etc: Walls of a room
╬: Doorway (Both horizontal and vertical)
Shown in #AA5500

-: Floor in a room
Shown in #55FF55

▒: Corridor
Shown in #AAAAAA

≡: Staircase
Shown flashing in black with #00AA00 background

◆: Trap
Shown in #AA00AA

*: Gold
Shown in #FFFF55

♣: Food
Shown in #AA0000

↑, ♀, ¡, etc: Items that can be picked up
Shown in #0000AA

$, +: Safe and perilous magic
Shown in #AAAAAA

#### Task 2. What Brogue improves on from Rogue

1. It gives much clearer information regarding what different objects do and how to use them

The UI is more clear over what effects items have, and what is currently happening.
When something is interacted with, text is shown at the top of the screen giving a description of what has happened.
when you see an object you have interacted with before, text is shown on the side describing what it is

2. It clearly shows areas you can see and ones that are obstructed from view

In "Rogue", enemies would just appear if they've entered your view, all rooms are shown in the same way.
In Brogue, walls, floors and items are slightly dimmed if they cannot be seen, when an enemy enters the players view,
a message is shown to inform the player, and the movement will temporarily pause

3. The game does a better job of showing how to interact with the Environment/Menus.
   Keybindings are shown for every option, meaning there isn't any confusion regarding opening the inventory or using an item

#### Task 3. Sample other Rogue game

I played NetHack, I found a few differences between it and the other two rogue games.

1. The rooms are revealed based on your view, similar to Brogue. however they are still connected by specified passages
2. The player doesn't auto pickup items, unlike the other two. Doors also must be opened manually.
3. The map generations are more focused on passages, rooms are spread further apart generally. They also appear to be able to have multiple staircases, whilst I could only ever find one in Rogue/Brogue

### Journal 2 30/09/26

#### Notes

##### Demo 1

Functions are declared in the same way as C#

std::cout << "Hello World!" is the equivalent of

```bash
echo "Hello World!"
```

```Cs
GD.Print("Hello World!");
```

Functions must be declared before they can be used, Similar to python.

#### Demo 2

You can split the declaration and body of a function.

write the function at the top of the screen with a semicolon

```c++
Type fooBar();
```

Now, rewrite it later with the body and without the semicolon. the compiler will pick up the declaration and no longer show an error.

```c++
std::string fooBar();

int Main()
{
     std::cout << fooBar()
     return 0;
}

std::string fooBar()
{
     return "Hello World!";
}
```

#### Demo 3

Header files (\<File>.h) contain #includes, namespaces and declarations.
Other scripts can then include it with

```c++
     #include "<File>.h"
```

Which will give it access to all includes and declarations made in the header files.

```C++
// MathHelpers.cpp

#include "MathHelpers.h"

int Square(int value)
{
     return value * value;
}

int Double(int value)
{
     return value + value;
}
```

```c++
// header.h

#ifndef MATHHELPERS_H
#define MATHHELPERS_H

int Square(int value);
int Double(int value);

#endif // MATHHELPERS_H
```

```C++
// Main.cpp

#include "MathHelpers.h"

int Square(int value)
{
     return value * value;
}

int Double(int value)
{
     return value + value;
}
```

#### Demo 4

```c++
#include "<library>"
```

Does not create a link between the two files, or similar. It simply copies all of its code into the current file when compiling.

I precompiled the code and output the contents to test.txt. The file was ~44k lines long, with my Main() function placed at the bottom.

#### Demo 5

to prevent a header being read multiple times by the compiler, use:

```c++
#ifndef HEADER_H // If this header has been read before, skip to #endif
#define HEADER_H // Define the header

...

#endif // Skip to here!
```

#### Demo 6

Angle brackets <>:
search for header in the system and library folders

Quotes "":
search for header in the project folder, then check system if nothing found

#### Demo 7 & 8

If a function is declared, called but never defined, you will receive an error.

On the other hand, If a function is defined more than once, you will receive an error as the
compiler doesn't know which to use.

header files should only contain declarations, as you can have multiple of those without receiving an error. but if you put a
function into it, then that function will be copied several times into each folder and give you the error above.

Other notes:

Headers:

1. Only function Declarations.
2. Structs and Classes should go into Headers
3. Constants are shared between files using the header.
4. Should have an Include guard for safety

Source Files:

1. Function Definitions
2. Everything needed by only that file should go into a .cpp
3. Includes only needed by that file
4. Shouldn't have a guard

##### Correct header format

```c++
#ifndef FILENAME_H
#define FILENAME_H

// #includes that this header itself needs
//
// declarations

#endif // FILENAME_H
```

##### C++ Build process

_1. Preprocess_: Swap out all `#Includes` for their original contents  
_2. Compile_: Translate .cpp to .obj  
_3. Link_: join all .obj files into one executable

#### Final Notes:

The compiler only runs once, from top to bottom. If a function is used before being defined, there will be an error.

Functions can be declared multiple times, But only defined once. Hence:

A functions declaration should go into a Header, It's Definition should be in a Source file (.cpp)

#Include simply pastes the files contents. You need guards to prevent any Errors.

### Journal 3, 1/10/26

#### Task 1, Sort lines into correct order

##### Original:

```C++
'A' #include "Inventory.h"
'B' int SlotsUsed(int items);
'C' #endif // INVENTORY_H
'D' int SlotsUsed(int items) { return items; }
'E' #include <iostream>
'F' #define INVENTORY_H
'G' int main() { std::cout << SlotsUsed(4) << "\n"; return 0; }
'H' #ifndef INVENTORY_H
'I' int SlotsUsed(int items) { return items; } (a second copy of the body)
'J' #pragma once
'K' #include "Inventory.cpp"
'L' int SlotsUsed(int items); (a second copy of the declaration)
```

##### Attempt:

```C++
'_Inventory.h_'
'F' #define INVENTORY_H
'H' #ifndef INVENTORY_H

'B' int SlotsUsed(int items);
'C' #endif // INVENTORY_H

'_Inventory.cpp_'
'J' #pragma once
'K' #include "Inventory.cpp"

'D' int SlotsUsed(int items) { return items; }

'_main.cpp_'
'A' #include "Inventory.h"
'E' #include <iostream>

'G' int main() { std::cout << SlotsUsed(4) << "\n"; return 0; }

'_Unused_'
'I' int SlotsUsed(int items) { return items; } (a second copy of the body)
'L' int SlotsUsed(int items); (a second copy of the declaration)
```

##### Solution:

```C++

'_Inventory.h_'
'H' #ifndef INVENTORY_H
'F' #define INVENTORY_H

'B' int SlotsUsed(int items);

'C' #endif // INVENTORY_H

'_Inventory.cpp_'

'D' int SlotsUsed(int items) { return items; }

'_main.cpp_'
'A' #include "Inventory.h"
'E' #include <iostream>

'G' int main() { std::cout << SlotsUsed(4) << "\n"; return 0; }

'_Unused_'
'J' #pragma once
'I' int SlotsUsed(int items) { return items; } (a second copy of the body)

'_Traps_'
'K' #include "Inventory.cpp"
'L' int SlotsUsed(int items); (a second copy of the declaration)
```

### Note:

The definition script (Inventory.cpp in this case) does not require an include to the header where it is defined. As long as the functions have the same name and parameters, it will work.

#### Task 2, Error causes

_1> error C1083: Cannot open include file: 'Invenotry.h': No such file or
directory_:

Compiler Error, as it starts with C#### ✓ (Preprocessing strictly)  
The error comes from misspelling Inventory.h ✓  
The file which #includes Inventory.h, main.cpp ✓

_1> error C3861: 'SlotsUsed': identifier not found_:

Compiler Error, as it starts with C#### ✓  
The error comes from SlotsUsed() not being defined, So it is likely there is misspelling. ✓  
(OR, it is declared after it's called, OR it's missing)

the file which defines SlotsUsed ✓

_1> main.obj : error LNK2019: unresolved external symbol "int \__cdecl
SlotsUsed(int) "
1> referenced in function main_:

Linker Error, as it starts with LNK. ✓
Declared but never defined, OR definition and declaration do not match ✗  
Inventory.cpp, Check that the definition is present. ✗

_1> error C2011: 'Item': 'struct' type redefinition_:

Compiler Error ✓  
Item, of type Struct has been defined multiple times. ✓ (header has no guard)
The file which defines Item ✓

_1> Inventory : error LNK2005: "int \__cdecl SlotsUsed(int)" already defined in
main.obj_:

Linker Error ✓  
The function is defined in two .cpp files OR it's in a header and pasted twice
Both files named ✗

#### Task 3, Create new SlotsFree() func

##### main.cpp

```c++
std::cout << SlotsUsed(4) << "\n";
std::cout << SlotsFree(4) << "\n"; ✓
return 0;
```

##### Inventory.h

```c++
int SlotsUsed(int items);
int SlotsFree(int items); ✓
```

#### Inventory.cpp

```c++
int SlotsFree(int items) ✓
{
     return 10 - items;
}
```

#### 3 Deliberate Errors

_Removing SlotsFree() declaration from Inventory.h_: error 'SlotsFree' was not declared in this scope ✓

_Remove definition from Inventory.cpp instead_: undefined reference to 'SlotsFree(int)' ✓

_Swap the parameter of SlotsFree to a float_: undefined reference to `SlotsFree(float)' ✓

#### Task 4, Missing guard

##### Part 1, Hand Precompile includes

Code:

```c++
// <iostream> would go here.
struct Item
{
     int weight;
}; ✓

struct Item
{
     int weight;
}; ✓

int TotalWeight(int itemCount);

int main()
{
     Item sword;
     sword.weight = 5;
     std::cout << sword.weight << "\n";
     return 0;
}
```

##### Part 2, Predict error

An error would occur due to `Item` being declared twice, both main.cpp and Inventory.h include it, thus it gets pasted twice. The error shown is "error: redefinition of ‘struct Item’" ✓

##### Part 3, Fix the error

The error can be fixed by removing the #include for Item.h in main.cpp, such that it looks like: ✗

```c++
#include "Inventory.h"
#include <iostream>

int main()
{
     Item sword;
     sword.weight = 5;
     std::cout << sword.weight << "\n";
     return 0;
}
```

You should add a guard to Item.h to prevent any errors

##### Part 4:

_Inventory.h has no guard either, and you probably did not need to
add one to make this build. Should you add one anyway? Give a reason._

You should just in case you reuse Inventory.h later, but you should remove it when shipping the project if it wasn't used ✓

It prevents an error that can be annoying to debug, So allways add it just in case.

#### Task 5, Predicting paste

```c++
// Colours.h
#ifndef COLOURS_H
#define COLOURS_H

struct Colour
{
 int red;
 int green;
 int blue;
};

#endif // COLOURS_H

// Palette.h
#ifndef PALETTE_H
#define PALETTE_H

#include "Colours.h"
int BrightnessOf(int red, int green, int blue);

#endif // PALETTE_H

// main.cpp
#include "Colours.h"
#include "Palette.h"
```

_1. In what order will the compiler paste the includes?_:

It will first paste Colours.h into main.cpp  
It will then paste Palette.h int o main.cpp  
It will try to paste Colours.h, But will see that it has already been defined, so it will skip to the end of its file and paste nothing. ✓

_2. How many times does the definition of struct Colour end up in the translation unit?_:

It should only end up there once, as the compiler is responsible for pasting the #includes, and is what skips over the #ifndef conditions. ✓

_3. swap the two #include lines in main.cpp so that Palette.h comes first. Does the program still build?_:

Both headers have guards against duplicate definitions, so an error shouldn't occur. ✓

Well-placed guards prevents errors stemming from swapping #Include orders, Always have them to prevent random bugs.

_4. Delete the three guard lines from Colours.h only, and predict the error before building_:

As Colours.h is included after Palette.h, there will be an error due to multiple definitions. ✓

'Colours.h: C2011: 'Colour': 'struct' type redefinition'

_Error_:

'error: redefinition of ‘struct Colour’  
1 | struct Colour' ✓

#### Task 6, Where does it belong?

_For each item, say whether it belongs in the header, the source file, or either, and give a one-line reason_:

1. int MaxSlots(int level);
   Either, it is fine for a Source File to contain a declaration. ✗ Header, Declarations go into headers

2. The body of MaxSlots.  
   Source file, if it is in a header, it may be duplicated several times. ✓

3. struct Item { int weight; };  
   Source file, if it is in a header, it may be duplicated several times. ✗ Structs go int headers.

4. #include <iostream>, needed only by one function's body.  
   Either, preferably the include should be put into the functions File for clarity, but it won't cause problems ✗  
   Source, Put it where it is needed otherwise files that don't need it will be force to have it.

5. #include "Item.h", where the header itself mentions Item.
   Header, ✗ A header should be self sufficient, you shouldn't need to guess what is needed.

6. #ifndef INVENTORY_H
   Header, this is used to add a guard onto a header and prevent duplicate definitions ✓

7. A helper function that no other file will ever call.
   Source file, it isn't needed else where so it shouldn't be included ✓

8. int main()
   Source file, Headers should contain declarations only. ✓

### Journal 4, 02/10/26

#### Task 1

##### 1.

Write 13 in binary:

1101 ✓

Write 255 in hexadecimal:

FF or 0xCCCCCC ✓

0xFF

##### 2.

A 4-byte int holds upto:

4.6 million (2.3 Million signed) ✗

4 Billion (3 Billion signed)

Why is the negative limit one further from zero:

Integers has an even number of possible values, Zero is used, making the possible (Non-zero) values odd.  
Negative sign is given an extra number as the positive side has zero. ✓

##### 3.

What happens when you add 1 to the largest int:

It wraps around ✓
-2 Billion

##### 4.

sizeof():

char: ✗ 1 byte
int: ✗ 4 bytes
double: ✗ 8 bytes

##### 5.

A function calls another, what is created, what is destroyed when it returns:

✗ A stack frame, parameters, locals, return address is all stored and deleted.

##### 6.

What does stack overflow mean, what causes it usually in your own code?:

✗ Lack of condition on a recursive function, causing it to loop infinitely until no memory left

#### Pointers

pointers are addresses in memory.
They are a variable that holds a location, rather than a (informative) value.

Example:

```c++
int playerHealth{ 40 };
int* healthPointer{ &playerHealth };

*healthPointer = 25; // Modified playerHealth through pointer
```

& refers to a variables address, rather than it's value.

```c++
*pointer = variable
// the value of the variable which pointer points to is modified, opposed to pointer now referencing variable.
```

call function on pointer:

````c++
(*pointer).Function(); // Old, never used
pointer->Function(); // Use this instead

// example:
target->Describe();
target->Health -= 4;

Pointers may have a null value, not referencing any variable. Check they arent null with

```c++
if (*pointer == nullptr)
````

pointers are similar to C# reference types.

Order

```c++
const int* p // pointer to constant int
int* const p // constant pointer to int
```

### Journal 5, 03/10/26

#### Point and Colour

##### Point.cpp

_Operator-_:

The minus operator for a point is simply:

```c++
return { x - rhs.x, y - rhs.y };
```

_DistanceTo_:

The code for the DistanceTo function is:

```c++
float Point::DistanceTo(const Point& target) const
{
    int dx = x - target.x;
    int dy = y - target.y;
    return std::sqrt(static_cast<float>(dx * dx + dy * dy));
}
```

#### Build debug

Originally Rider wasn't entering debug mode properly, It would ignore Breakpoints and asserts. After recreating the CMake project it began to work properly.

#### Final notes:

##### DistanceTo:

The DistanceTo function calculated to difference in x and y to a given target, called Dx and Dy respectively.
Both Dx and Dy where raised to the power of 2, added and then square rooted. This gives us the correct distance between two points.

##### Testing

I tested that the final code worked by adjusting the values for the characters position and colour.
Adjusting the playerLocation variable and the colour put into the console showed that the code was no longer Hardcoded to put the player in a specific position with a specific colour.

This means we will be able to move the player around easily. However, I feel we should first create a new class for a block, which contains a point and colour, and provides functions which allow for all objects in a scene to be iterated over and drawn.

### Journal 6, 05/10/26

#### Three additions to add movement to the @

1. Add `Input InputHandler` to Engine.h
2. Add to`Engine::HandleInput()`:

```C++
void Engine::HandleInput()
{
    inputHandler.CheckForEvent();
}
```

3. Add movement and bounds logic to `Engine::Update()`:

```c++
    Point delta{Point::Zero};

    switch (inputHandler.GetKeyCode())
    {
    case SDLK_UP:
        delta = {0, -1}; // up is one row less
        break;
    case SDLK_DOWN:
        delta = {0, 1};
        break;
    case SDLK_LEFT:
        delta = {-1, 0};
        break;
    case SDLK_RIGHT:
        delta = {1, 0};
        break;
    default:
        break; // any other key: no movement
    }

    Point newLocation{playerLocation + delta};
    bool inBounds{ (newLocation.x >= 0 && newLocation.x < screenWidth) && (newLocation.y >= 0 && newLocation.y < screenHeight) };

    if (inBounds)
    {
        playerLocation = newLocation;
    }
```

#### How does the use of SDL_WaitEvent make the Rogue-like different to a platformer?

Our game will only process updates for other entities (For example, Enemies) once the player has moved.

#### Lab 05

_In Part D you broke Point and a test caught it. How is that different from the way you would have
found the same bug in Lab 3?_:

In lab 3 we would have placed a temporary `Assert()` case in our `Engine::Render()` function, this would have been slower, harder to setup more cases for, and wouldn't have provided as clear of information as using separated TestCases is.

_Why did the distance test need Catch::Approx() when the addition test did not? What is different
about the two return types?_:

In case of floating point imprecision, which may lead to improper test fails, which result from no changes to the code.

_We tested Point but deliberately did not unit-test Input or Render. What makes those two harder to
test this way?_:

Those both are part of the game loop, It would be much more difficult to setup and print out helpful information for those parts of the game as they require either user input or visuals.

### Journal 7, 05/10/26

#### Questions

1. &thing references the variables address ✓  
   *ptr acts as a reference to a variable ✓

2. Is *ptr null? ✓ is it nullptr?

3. SDL is C, and thus has no references

4. It can test cases without launching the full game, making it faster. ✓ Can setup tests

5. if the test code is correct, it only states that the tests were passed ✓

6. in case of floating point imprecision, which may lead to fails despite no changes to the test code. ✓

#### Problem 2

1. Same value, a two separate copies of an engine will work the same, Seeds are designed such that they always produce the same values ✓
2. Different Values ✓ Seeds work such that they will always generate random values. if the value is incremented by one, A completely seperate value is generated.
3. Same values ✓ The engine is copied, creating a separate engine with the same values
4. Different Values ✓ Two draws from one engine, the engine moves on
5. Different Values ✓ Different distributions ,different values can be drawn
