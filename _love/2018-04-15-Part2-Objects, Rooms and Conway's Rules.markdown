---
layout: post
title:  "Part 2: Objects, Rooms and Conway's Rules"
date:   2018-04-14 17:09:12 +0600
author: Mikhail Adamenko
image: "/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/Cell_Rules.png"
---

### Previously
!["Previosuly"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/Header_Previously.png "Previosuly")

In the [previous part](https://mikhailadamenko.design/love/2018-04-14-Part1-installing-and-launching.html) of the tutorial, we installed the framework and make a minimal configuration to it.

Right now, when we have a basic structure, we expand it with objects and libraries, so our final code of the game wasn’t written in one main file and have a good and shiny order.

### Conway's Game of Life
I definitely should have explained `Conway's Game of Life` earlier, but later is better than never!

Creator of the game is [John Horton Conway](https://en.wikipedia.org/wiki/John_Horton_Conway), the game has not required any inputs from a player, at least in the classic version of it, but you always can create own rules and universe where interaction with the cells is possible.

In the universe which is a mostly two-dimensional grid (x and y), created cells have two possible states: alive (1) or dead (0). Every tick or evolution each cell interact with their eight neighbors. 

A cell uses the following rules to calculate next generation:
!["Cell rules"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/Cell_Rules.png "Cell rules")

1. Any live cell with fewer than two live neighbours dies, as if caused by underpopulation.

2. Any live cell with two or three live neighbours lives on to the next generation.

3. Any live cell with more than three live neighbours dies, as if by overpopulation.

4. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

The rules above repeatedly applied to create next generations.

The game is simple enough, but it still has a beauty in patterns of life it generates each generation. I will show a "still life" pattern which is a most common in the game, other patterns you can find [here](http://conwaylife.com/wiki/Category:Patterns).

!["Still lifes"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/Still_Life.png "Still lifes")

### Objects

!["Objects"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/Header_Objects.png "Objects")

At the moment we know the rules of our game, and we have an initial structure which is the two files `main.lua` and `conf.lua`. We can start to organize our code, to do this we will use objects that saved in separate files, each does their own particular job. 

In order to do it, we can use a library called [Classic](https://github.com/rxi/classic), it is the module for creating objects and classes in the Lua, or you can find an OOP library that you liked.

Although Lua hasn't classes or objects, they still can be created without any libraries. I will show both ways of creating objects, first is a pure Lua and second with the library. Lua implementation goes first, so feel free to skip it and move to the second part.

The code below you can write in a separate from the game file or in the Lua terminal environment, but to do any of the Lua programming separate from the framework LÖVE you need to install the [Lua](https://www.lua.org/) itself, which is pretty simple.

**And as always code written in the tutorial will be available at the end.** 

### Lua Object

To have own object implementation, first, you need to delcare a table with the name of it:

    local Text = {}

after, define the object:

    -- the "newObject" name not required, 
    -- you can call it whenever you like.
    function Text:newObject()
        -- Here goes constructor code
        local object = {}
        object.text = "Hello! "

        -- local variables inside the object
        -- are not available outside it
        local greetingStr = ", glad to see you!" 

        -- a method of the object that uses colon ':' 
        -- are available from the outside
        function object:sayHello(name)
            print(self.text .. name .. greetingStr)
        end

        -- a method with the dot is available
        -- only in the object itself
        function object.increment()
        end

        -- The destroy method is purely optional 
        -- because of the Lua garbage collector, 
        -- but still useful to have, so you can be sure 
        -- that every object we created will be destroyed
        function object:destroy()
            self.text = nil
        end

        return object
    end

As we created our object, we can call it, declare and define a variable with the call of the object:
    
    local sentence = Text:newObject()

Call the method `sayHello` which takes a string type:

    sentence:sayHello("Mikhail")

Result will be: `Hello! Mikhail, glad to see you!`. 

You can play with the object by yourself, write the code above in a separate file and call it from the terminal:

    lua object.lua

This is, of course, is not a full implementation of the object, I do not mention an inheritance, and export, and many more, but the example above may give you a nice little help for further expanding the object.

### The Library Object

Download the [classic.lua](https://raw.githubusercontent.com/rxi/classic/master/classic.lua), create a new folder to where you will save all the libraries that be used in the game and put the file inside it. 

I’ve created folder `libraries` where all my libraries have their own folders because some of them can have more than one file, in my case a full path for the `classic.lua` will be:

    ~/love/GOF/libraries/classic/classic.lua

After you saved the file, require it into the game, by writing in `main.lua` at the top:

    Object = require "libraries/classic/classic"

Where the last `classic` is our file without `.lua` extension, while previous are folders, `Object` is the global variable that we will use for accessing libraries functional, you can write any name you like.

In the root of the game create folder `objects`, in the folder, you can save all the object files for the game, the folder isn't required but good to have so everything can be in the order. 

Inside the folder create file `Text.lua` which will be our file for the object `Text`. Every object you created need to extend `Object` from the library `Classic` so we can treat it as the object, to do it, write at the beginning of the file:

    Text = Object:extend()

By extending the library object, every file in the game can have access to it. After initializing the object, create a method `new()` of the object:

    function Text:new()
        -- the body of the method
        -- available outside it
    end

Inside declare and define two variables we wrote before:

    self.text = "Hello! "
    self.greetingStr = ", glad to see you!"

The variables without `self` will be global, which is useful if you have a small project, but will be nightmarish for the middle-to-big game, then global variables in the object can lead to a [name collision](https://en.wikipedia.org/wiki/Name_collision), so better to use local variables. 

The `new()` used here as the main method where we store our variable and call other methods of the object in the first call.

Next, define method `sayHello` with an argument `name`, inside the method print to the terminal a sentence you want, by using local variables and the argument:

    function Text:sayHello(name)
        print(self.text .. name .. self.greetingStr)
    end

At the end let's create a `destroy()` function, which is again not required, but in the future will help you, so you can say to a garbage collector which items is no longer in use:

    function Text:destroy()
        self.text = nil
        self.greetingStr = nil
    end

The last step before launching, we need to require our object in the `main.lua`, requiring an object is similar to the libraries after `Classic` write:

    require "objects/Text"

Where `objects` is the folder and `Text` is the `Text.lua` file. 

In the `love.load` function, call the object we created:

    function love.load()
        -- call the object in the main file    
        local sentence = Text()
        
        -- print greeting text with different names
        sentence:sayHello("Mikhail")
        sentence:sayHello("Josh")
    end

Finally launch the LÖVE game, in the terminal you should see two sentences with different names, or alternatively you can use LÖVE's [graphics.print](https://love2d.org/wiki/love.graphics.print) function to display the text on the game screen, to do it you need to remove print in method `sayHello(name)` and replace it by `return`:

    return self.text .. name .. self.greetingStr

and in the `love.draw` function print our text:

    function love.draw()
        love.graphics.setColor(1,1,1)

        love.graphics.print(sentence:sayHello("Mikhail"), 10, 10)
        love.graphics.print(sentence:sayHello("Josh"), 10, 30)
    end

This is it, the self-written object and the library, you can use any variants you like, it's up to you, in my game and tutorials I will be using the `Classic's` approach of creating objects.

### Room

!["Room Part"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/Room.png "Room Part")

**What is room?**

In every game you can see a bunch of different screens, like menu screen, setting screen, game screen, authors, and more. Here where scene manager will come, every game engine has it, for example in [Unity](https://unity3d.com/) it is a scene where you place all your game objects, scripts, and other parts related to the current [scene](https://docs.unity3d.com/Manual/CreatingScenes.html) items.

Room size can be bigger than a window screen in this situation we can choose our camera behavior, it either, fixed at one place, which means that we see only inside the window border, and manage to change camera view when a player hit a trigger—moves outside a window border—or follow our player, of course, there are many other combinations of the camera, there we limited only by programmer imagination, for example, game "Shovel Knight"

<iframe width="auto" height="400" src="https://www.youtube.com/embed/jo-uuawy9Ok" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Uses the combination of the camera view, it can show only part of the big room, while stay fixed, or follow the player, unfortunately in a `Game of Life` camera always stays fixed, so camera implementation is on you.

In the room every item is a game object, Unity engine also has own [game object](https://docs.unity3d.com/Manual/GameObjects.html) definition, shortly—every item in a room is a game object that may or may not be interactive, for example, an enemy, a chest, a background image, or a sound is game objects.    

**Why do you need room?**

1. To load and unload screen with objects inside them
2. To switch between scenes

In the room, we will create or call items related to it, for example, take a Menu room, in the room, we are probably want to have:

1. Buttons for changing a room to a game or settings, 
2. Button for quitting the game,  
3. Backgrounds, 
4. Sounds, 
5. Special effects and many more other items.

And when the room change happens all items that were in the previous can be safely destroyed, in order to free allocated space and avoid functional crossing, for example, when one button bind to do different operations or in my case a table with items in it transferred between rooms.

**Make a room**

So, if you want to have a game with different screens, then you need to store information about it and items in it, in a separate file, for clarity sake of course! In order to have a room, create the folder `rooms` inside the `objects` folder, this way you will have a track of all rooms in the game, it is not necessary but good to have.

Inside the `rooms` folder create `Menu.lua`, the object which will be our main menu room in the game, from where we can go further to the game or options room.

In the `Menu.lua` define `Menu` as the object:

    Menu = Object:extend()

The standard layout for all our objects in the LÖVE framework, will have a "new," "update," "draw," and "destroy" methods:

    function Menu:new()
    end

    function Menu:update()
    end

    function Menu:draw()
    end

    function Menu:destroy()
    end

Now let's draw a circle in the "Menu" room, so we can see a difference when we change room from one to another, the first change is a background color, from black to white, why do this I explain a little bit later:

    love.graphics.setBackgroundColor(1,1,1)
    
This will change entire screen background to white, next, so we can have our circle in the center of the screen, define two local variables with a width and height of the window:

    self.screenWidth    = love.graphics.getWidth()
    self.screenHeight   = love.graphics.getHeight()

[graphics.getWidth](https://love2d.org/wiki/love.graphics.getWidth) and [getHeight](https://love2d.org/wiki/love.graphics.getHeight) takes visible window width and height in pixels—without any panel bars—at the moment of calling.

Now on drawing the circle, in the `draw` method change the color of the circle, I choose blue:

    love.graphics.setColor(0.258, 0.525, 0.956)

After color change, create the circle by using [graphics.circle](https://love2d.org/wiki/love.graphics.circle):

    love.graphics.circle("fill", self.screenWidth / 2, self.screenHeight / 2, 100)

The first argument—define the method of drawing geometry object, `fill` well, will fill a shape with the color, while `line` will draw only the border of a shape. 

The second and third arguments—is x and y position of the circle, I take half of the width and height of the window so the circle will be on the center screen. 

The fourth argument—is a radius of the circle. 

To see our circle, we need to change a room from the `main.lua` to the `menu.lua`—`main.lua` actually not a room, but LÖVE framework starts with this file, so basically it's a room and not at the same time—in order to change a room we can call it directly in the `load` function, but this is not so interesting, instead we make `F1` key to be our trigger for changing room. 

In the `main.lua` require our `Menu` object:

    require "objects/rooms/Menu"

BTW, all previous experiments with the `Text` object you can delete, so at the end, you will have empty `main.lua` with two requires, `Object` and `Menu`.

Now make a new function [love.keypressed(key)](https://love2d.org/wiki/love.keypressed):

    function love.keypressed(key)
    end

Write in the function an "if" condition, if `F1` was pressed - call the `Menu` object, and print a message to the terminal; I'll later explain why we need the message:

    if key == "f1" then
        Menu()    
        print("The Menu room created!")
    end

Right now, launch the game and look at the result, it should be at first a black screen which is our Schrödinger room, that after pressing the "F1" button will change to a white screen, this is our Menu room, but wait, where is the circle? 

What goes wrong? When you call the "Menu," it was loaded, but only with `new` method that has two variables and background redraw, every other method outside `new` is not called, to fix it we will write a function for changing the room with the call of all its methods. 

To do so, return to the `main` file and define global variable `current_room` with value `nil` in the `love.load`:

    current_room = nil

This variable will hold string value of the current active room. In the `love.update` write a condition that if the current room exists then activate `update` method of it:

    if current_room then
        current_room:update(dt)
    end

In the `love.draw` the same, activate `draw` method if we have the room:

    if current_room then 
        current_room:draw() 
    end

Almost done! Define a new function `gotoRoom` with the arguments of `room_type` and `three dots (...)`, which will make it the [variadic function](https://en.wikipedia.org/wiki/Variadic_function)—usual function but instead of the defined amount of arguments, this can take any—we will not use additional arguments, instead we will use a [Vararg](http://lua-users.org/wiki/VarargTheSecondClassCitizen) in the definition of the `current_room` this way we can call methods of the object:

    function gotoRoom(room_type, ...)
    end

Inside the function, define `current_room` variable with an argument of `room_type` which is a string type of our rooms, for example, `Menu.lua` will be just a `Menu` or `Game.lua`-`Game` and so on:

    current_room = _G[room_type](...)

[_G](https://www.lua.org/pil/14.html) in the Lua is the table with global variables, you can see all of them by printing in the terminal:

    for n in pairs(_G) do print(n) end

and if you add [room_type] then you will see all our methods of the room:

    for n in pairs(_G[room_type]) do 
        print(n)
    end

At last we still have a `destroy` method which not used for now, but still be good to call for future use:

    if current_room and current_room.destroy then 
        current_room:destroy() 
    end

Function `gotoRoom` after all these manipulations should look like this:

    function gotoRoom(room_type, ...)
        current_room = _G[room_type](...)

        if current_room and current_room.destroy then 
            current_room:destroy() 
        end
    end

The last drop is to replace directly call of the object `Menu` by function we created:

    gotoRoom("Menu") 

As for now you can change the room and see the circle that previously wasn't displayed, try it!

!["F1-Keypress"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/f1.png "F1-Keypress")

At the moment of creating the menu room, I wrote the print message and promise that explain what its needs, the time has come! In the game, change the room to the menu as you usually do, but now! Smash the key again-and-again, what you will get is the same message in the terminal every time you hit the key, although nothing is happening and you no more in the `main` room, so why you still have it?

The answer is simple, each object that we have will compile in the `main.lua` file, this way the functions in the file will have the global effect on every object.

The "global" functions, not only "keypressed" will prevent normal work in our game, especially if we want that the same item will have different actions in each room, or when no actions are needed.

In order to fix the "keypressed" problem [SSYGEN](https://github.com/SSYGEN) wrote a little library called `Boipushy` which will help us, how to use and the library file you can find [here](https://github.com/SSYGEN/boipushy), now let's rewrite interaction with the button, download [input.lua](https://github.com/SSYGEN/boipushy/blob/master/Input.lua), place it in the library folder and require from the `main.lua`:

    Input  = require "libraries/boipushy/input"

I saved the file in "boispushy" folder, at this point you already know how to call library and object files, so feel free to make your own file hierarchy.

To change input from the `keypressed` to the library, in the `love.load` write:

    input = Input()

    input:bind("f1", function()
        gotoRoom("Menu")
        print("The Menu room created!")
    end)

Here we declare `Input` object into the global `input` variable, then to the `F1` key bind the change of the room action and the message print. 

As `SSYGEN` write in his documentation for the library, you can declare `Input` object for other players, for example, `input` variable will be in use for the main player, while:

    secondaryInputPlayer = Input()

    secondaryInputPlayer:bind("f1", function()
        -- Hi, I'm the second controller!
    end)

for the second player.

Don't forget to entirely remove function `love.keypressed(key)`, then in the `menu.lua` add to the `new` method unbind function for the key we use:

    input:unbind("f1")

These way you remove all the bound functional of the key, this is it! Smash the button wherever times you like see the result.

!["The End"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/The_End.png "The End")

For this part of the tutorial I think it's enough information to have, try to play with different keys, rooms, object, libraries, in the next part we continue with the rooms, game objects, areas, we will make menu and a main game screen.

The first half of the tutorial files:
[object.lua](https://github.com/A-Mikhail/Love-GameOfLife/blob/tut-part2/object.lua),
[main.lua](https://github.com/A-Mikhail/Love-GameOfLife/blob/tut-part2/main.lua), [Text.lua](https://github.com/A-Mikhail/Love-GameOfLife/blob/tut-part2/objects/Text.lua) 

The second half:
[main.lua](https://github.com/A-Mikhail/Love-GameOfLife/blob/tut-part2-1/main.lua),
[Menu.lua](https://github.com/A-Mikhail/Love-GameOfLife/blob/tut-part2-1/objects/rooms/Menu.lua)

### TL;DR
In the article was explained what is Conway's game is, in short it is evolution simulator where a cell has condition live or dead, 1 and 0 respectively, evolution happens each tick by applying rules to a cell, next was created object `Text` in the Lua, by using self-written and with the library `Classic` ways.

At last, we created `Menu` room in which we draw a big blue circle at the center of the screen, without any good reason to do it, finished with a few examples of keypress and problem it can have. 

### Before you move next
I want to thank you for reading the article, hope you find it interested to read, if not or you have something to say, please leave a comment or write directly to [me](https://mikhailadamenko.design/about). I will highly appreciate every feedback you have.

*Have a good day!*