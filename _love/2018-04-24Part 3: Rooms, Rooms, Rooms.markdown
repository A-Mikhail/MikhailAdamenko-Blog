---
layout: post
title:  "Part 3: Rooms, Rooms, Rooms!"
date:   2018-04-24 15:41:55 +0600
author: Mikhail Adamenko
image: "/assets/love-tutorials/images/Rooms_Rooms_Rooms/Thumbnail_for_Part3.png"
---

### In the tutorial
Hello, I'm Mikhail, in this part of the tutorial we will write functional Menu room with UI in it, as well as Game room in which our gameplay will happen, in the game room we will have a player as a game object. In the end, we will have an overall structure for making the game, and from which I will show how I made Conway's Game, but this will be in the next tutorial, for now, we just finish the general structure. If you have any questions about this part of the tutorial, please leave a comment.

### Menu Room

!["Menu Room"](/assets/love-tutorials/images/Rooms_Rooms_Rooms/Menu_room.png "Menu Room")

In the previous [tutorial](https://mikhailadamenko.design/love/2018-04-15-Part2-Objects,%20Rooms%20and%20Conway's%20Rules.html), our menu wasn't the best what we can think of we have not any buttons as any proper menu usually should have instead we have some blue circle in the center of the screen! 

Maybe to someone the circle can be useful for the background image, but not for us! 

To get rid of the circle or not I will leave up to you at this point of the tutorial you should have some knowledge of how the framework works, as for now, let's add some buttons and actions to them. We will have two buttons Start and Quit, start button will transfer us to the Game room—which we will create in the tutorial later—while quit just quit the game, nothing fancy here.

For buttons, I will use a smallest, easiest and probably not the best library was chosen at the beginning of the making my "game," called [SUIT](https://github.com/vrld/SUIT), encourage you to try different libraries for the UI, some of them you can find in the [awesome-github-list](https://github.com/love2d-community/awesome-love2d#ui) for the framework. I took the SUIT because it was pretty simple, nothing extra like dropdowns or switches I have no needs. So, if you wanted to follow the tutorials then you're a brave on, follow next steps to have the library installed. To other who chooses a different library, follow steps that probably have the library.

This is where the tutorial begins, take a tea and move along it. First, clone or download zip file of SUIT library, unzip it and remove unneeded files from the repository like like `README.md,` `suit-0.1-1.rockspec,` and folder `docs`, remaining files place to the libraries folder of the game, I as always do place them in the folder called a `suit`. 

One thing I should mention, the library as any other written for the framework uses RGBA colors this works until the newest "11.0" update that changed all color functions to use only values in 0-1 range, because of this many libraries will be broken until you fix them manually, it not so hard, just take RGB range 0-255 and divide by 255, I already wrote this in the previous tutorials, but repeat will not harm. 

After you placed the library, call it from `main.lua`:

{% highlight lua %}
suit = require "/libraries/suit"
{% endhighlight %}

Write the `love.keypressed` function which we created in the previous tutorial and removed, inside the function forward keypresses to SUIT:

{% highlight lua %}
function love.keypressed(key)
    suit.keypressed(key)
end
{% endhighlight %}

In the `love.draw` draw the GUI:

{% highlight lua %}
suit.draw()
{% endhighlight %}

Go to the `Menu.lua` file, find `update` method inside it create button Start and Quit, with a statement that if the button is hit response with an action:

{% highlight lua %}
if suit.Button("Start", ).hit then
    -- do some action
end

if suit.Button("Quit", ).hit then
    -- quit the game
    love.event.quit()
end
{% endhighlight %}

This is just a structure for the button, not launch the game yet!
 
The [suit.Button](http://suit.readthedocs.io/en/latest/widgets.html#Button) I think is pretty obvious it calls button method where the first argument is a name of the button, then it has additional functional like a change of the font which should be in the table format after the first argument, this is useful but not we need right now, what we need is the position of the button, you can place each button by hands writing some values like 100, 200, after the name string, only this will be too much work if we have more than two/three buttons, better to create a [row](http://suit.readthedocs.io/en/latest/layout.html#row) from the library, for automatic arrangements, the row method takes width and height as the arguments, so let's make one after the comma in the start button write:

{% highlight lua %}
suit.layout:row(buttonWidth, buttonHeight)
{% endhighlight %}

The row will set positioning and size for the first and following buttons. In the second button write:

{% highlight lua %}
suit.layout:row()
{% endhighlight %}

It will automatically apply width and height of the previous button and put it below the first.

Inside the row parentheses, I wrote two variables `buttonWidth` and `buttonHeight`, define them outside the "if" statements in the `update` method, so we can rearrange button size when the screen size is changed:

{% highlight lua %}
local buttonWidth  = screenWidth / 4
local buttonHeight = screenHeight / 14
{% endhighlight %}

Here we again have two variable and some [magic numbers](https://en.wikipedia.org/wiki/Magic_number_(programming)), begin with the two variable `screenWidth` and `screenHeight`, they take a screen size of the window constantly, in the previous tutorial we wrote the same variable for circle positioning, this time we need to remove them because they take position of the window only when room is called, if you change the window size you get off-centered circle, to have the ability of resizing screen, in the `conf.lua` change value of `t.window.resizable` from false to true: 

{% highlight lua %}
t.window.resizable = true 
{% endhighlight %}

This time we define variables in the `menu.lua` inside the callback function [love.resize](https://love2d.org/wiki/love.resize), create the function and define variables:

{% highlight lua %}
function love.resize(w, h)
    screenWidth     = w
    screenHeight    = h
end
{% endhighlight %}

When the window resize we will update our coordinates so everything will be in the responsive state, right now we have all the variables we need for setting coordinates into buttons, first, we need set starting point where layout begin, to do this in the `Menu:update` method write [suit.layout.reset](http://suit.readthedocs.io/en/latest/layout.html#reset), I will set only first two arguments x and y: 

{% highlight lua %}
-- Reset or define the starting point
suit.layout:reset((screenWidth / 2) - (buttonWidth / 2), screenHeight / 2.5)
{% endhighlight %}

What you saw above by no means a good code and especially math, but for me, it's work so why to think more? 

Here I take half of the screen width and subtract from it half of the button width so the x point will be in the center of the button, with y position this will not work, because to the buttons we can add paddings and will add them; 

In theory, we can calculate overall occupied place of the buttons with all the paddings and find the center, but if you have two to three buttons this will not be worth the wasted time, just slam random numbers until you will be satisfied, again don't use the code above in the production, for prototyping it's ok. 

About [paddings](http://suit.readthedocs.io/en/latest/layout.html#padding), add them so our buttons will be placed loosely: 

{% highlight lua %}
-- Put extra pixels between cells in the y-direction
suit.layout:padding(0, 20) 
{% endhighlight %}

I added 20 pixels in the y-direction—top and bottom—for left and right I no need extra space because buttons already have a lot of space.

Finally! we can add our coordinates to the start button, write after the comma:

{% highlight lua %}
suit.layout:row(buttonWidth, buttonHeight)
{% endhighlight %}

and simple `row()`: 

{% highlight lua %}
suit.layout:row()
{% endhighlight %}

to each new button you want to have below previous, finished `Menu:update` should look like this:

{% highlight lua %}
function Menu:update()
    local buttonWidth  = screenWidth / 4
    local buttonHeight = screenHeight / 14
    
    -- Reset or define the starting point
    suit.layout:reset((screenWidth / 2) - (buttonWidth / 2), screenHeight / 2.5)

    -- Put extra pixels between cells in the y-direction
    suit.layout:padding(0, 20)  

    if suit.Button("Start", suit.layout:row(buttonWidth, buttonHeight)).hit then
        gotoRoom("Game")
    end

    if suit.Button("Quit", suit.layout:row()).hit then
        love.event.quit()
    end
end
{% endhighlight %}

Everything ready, launch the game and see your buttons:

!["Menu room buttons"](/assets/love-tutorials/images/Rooms_Rooms_Rooms/Menu_room_buttons.png "Menu room buttons")

The quit button even work! You can click and exit the game, try this incredible experience, while start button will tell you that no such room as "Game" is founded and crashed the game, to have such room you probably know what to do, add new object "Game" in the rooms folder, with expanding and basic structure of the room: 

{% highlight lua %}
Game = Object:extend()

function Game:new()
end

function Game:update()
end

function Game:draw()
end

function Game:destroy()
end
{% endhighlight %}

Don't forget to require it from the `main.lua`:

{% highlight lua %}
require "objects/rooms/Game"
{% endhighlight %}

Now every button we have in the menu room will work.

### Game Room, Area, and Game Objects

!["Game Room, Area, and Game Objects"](/assets/love-tutorials/images/Rooms_Rooms_Rooms/Game_room_area_and_game_objects.png "Game Room, Area, and Game Objects")

Moving to the game room, as examples, I will create a circle that would be our Player; which is a game object, add control functional, and make the game room infinite; actually game objects will spawn on the opposite side, while doing this tries to explain areas and game objects, at the moment when we will have fully functional `Area` and `GameObject` classes, we make an infinite room for each game object so the player can go from one side of the screen to the opposite, it's useful to have if you want to make looped gameplay.

To create GameObject class, in the folder `objects` create file `GameObject.lua` with default structure:

{% highlight lua %}
GameObject = Object:extend()

function GameObject:new()
end

function GameObject:update(dt)
end

function GameObject:draw()
end

function GameObject:destroy()
end
{% endhighlight %}

Every game object will have an area, x and y coordinates, and additional user-defined options, as the arguments in the `new` method add `area, x, y, opts`:

{% highlight lua %}
GameObject:new(area, x, y, opts)
{% endhighlight %}

Inside the method parse additional options if they are available if not object will have an empty table, create the local variable `opts` with the `opts` argument as value or empty table otherwise:

{% highlight lua %}
local opts = opts or {}
{% endhighlight %}

If additional options were provided then parse them:

{% highlight lua %}
if opts then
    for k, v, in pairs(opts) do
        self[k] = v
    end
end
{% endhighlight %}

So we can use them in the game object itself, how I will show later, for now, make local variables for the area, x, and y:

{% highlight lua %}
self.area = area
self.x = x
self.y = y
{% endhighlight %}

The `update` and `destroy` methods leave empty for now, add one new local method that will generate a unique id for each game object created, in order to differentiate them from one another:

{% highlight lua %}
function GameObject.UUID()
end
{% endhighlight %}

Inside it just copy-and-paste following code:

{% highlight lua %}
local fn = function(x)
    math.randomseed(love.timer.getTime())

    local r = math.random(16) - 1
    r = (x == "x") and (r + 1) or (r % 4) + 9
    return ("0123456789abcdef"):sub(r, r)
end

return (("xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx"):gsub("[xy]", fn))
{% endhighlight %}

The code above set random seed by using the [love.timer.getTime()](https://love2d.org/wiki/love.timer.getTime) function, then based on the seed generate 16 numbers and concatenate them in the template we make in `return`, generated UUID will look like this:

25219cdb-0906-4bb3-86b0-c05b62c62340

Finish the class with the call of our `UUID` method in the `new` write:

{% highlight lua %}
self.id = self.UUID()
{% endhighlight %}

Latest if you want, you can display ids of the game object in the `draw` method just for fun, write the following code:

{% highlight lua %}
love.graphics.setColor(0, 0, 0)
love.graphics.print("ID: "..tostring(self.id), self.x - self.x/2, self.y - 40)
{% endhighlight %}

This will display generated id of the player objects, when we create them, leave the code for now, but remove as you play enough with it. For now, `GameObject` class is finished.

### Areas
Area in the game needed for creating, drawing, updating and interacting with the game objects inside it, the areas have no needs if in the room you will not use any game objects, like in the menu room, the room will perfectly live without having an area or any game object at all, so think of it as the manager for game objects, to have it create `Area.lua`, inside the `object` folder, with default object structure:

{% highlight lua %}
Area = Object:extend()

function Area:new()
end

function Area:update(dt)
end

function Area:draw()
end

function Area:destroy()
end
{% endhighlight %}

In the new method declare argument `room`, which we will use for identifying its location, then inside the function create local variables for the argument room, and for all game objects inside the area:

{% highlight lua %}
function Area:new(room)
    self.room = room
    self.game_objects = {}
end
{% endhighlight %}

In area `update` we will call update method of game objects, to do this we need to take the individual object from the table "game_objects":

{% highlight lua %}
for i = #self.game_objects, 1, -1 do 
end
{% endhighlight %}

Inside the loop define the local variable for each game object we found:

{% highlight lua %}
local game_object = self.game_objects[i]
{% endhighlight %}

Then call `update` method of the object:

{% highlight lua %}
game_object:update(dt)
{% endhighlight %}

The finished `update` would look like this:

{% highlight lua %}
function Area:update(dt)
    for i = #self.game_objects, 1, -1 do 
        local game_object = self.game_objects[i]
        game_object:update(dt)   
    end
end
{% endhighlight %}

The `draw` method of the Area will call `draw` method of game objects:

{% highlight lua %}
for _, game_object in ipairs(self.game_objects) do
    game_object:draw()
end
{% endhighlight %}

Create method `:addGameObject` with the arguments of `game_object_type`, `x`, `y`, and `opts`. The function will insert game objects into the table we created in the beginning: 

{% highlight lua %}
function Area:addGameObject(game_object_type, x, y, opts)
end
{% endhighlight %}

Inside the function create local variables for additional options if they were provided and for the game object itself, with the value similar to `gotoRoom` function `current_room` variable, only this time instead of using vararg, initialize with the default or provided arguments:

{% highlight lua %}
local opts = opts or {}

local game_object = _G[game_object_type](self, x or 0, y or 0, opts)
{% endhighlight %}

Next, insert into `game_objects` table our `game_object`:

{% highlight lua %}
table.insert(self.game_objects, game_object)
{% endhighlight %}

And for `destroy` method, we will have identical functional as in update, but with one addition, we will remove objects from the table and call `destroy` method of the game object:

{% highlight lua %}
for i = #self.game_objects, 1, -1 do
    local game_object = self.game_objects[i]

    game_object:destroy()

    table.remove(self.game_objects, i)
end
{% endhighlight %}

Finish game object and area sections, with `require` from the `main.lua`:

{% highlight lua %}
require "objects/Area"
require "objects/GameObject"
{% endhighlight %}

This way we have basic structure for creating simple games, I encourage you to write in the comment section or directly to my [email](mikhail.adamenko@pm.me) if something wasn't quite right in the code or especially in the writing, it's my first attempts so I for sure make mistakes, and will be glad to have a response from you.

### Player

!["Player](/assets/love-tutorials/images/Rooms_Rooms_Rooms/Player_banner.png "Player")

Now when we have a minimal structure for having the game objects, let's create it. Create `Player.lua` file inside the `objects` folder and fill it with the default structure for all the game object you will have in the game:

{% highlight lua %}
Player = GameObject:extend()

function Player:new(area, x, y, opts)
    Player.super.new(self, area, x, y, opts)
end

function Player:update(dt)
    Player.super.update(self, dt)
end

function Player:draw()
    Player.super.draw(self)
end

function Player:destroy()
    Player.super.destroy(self)
end 
{% endhighlight %}

This is default structure for the game object classes, in our case for the `Player` class, first what we did is inherit the GameObject instead of just Object, next in the `new` method constructor we have four arguments: area, x, y, and opts, we send them to the GameObject or parent class by adding `.super.`:

{% highlight lua %}
Player.super.new(self, area, x, y, opts)
{% endhighlight %}

As well in the `new` method create two local variables for x and y:

{% highlight lua %}
self.x = x
self.y = y
{% endhighlight %}

An important part of the `Player` class is the player itself! In `draw` method create a circle:

{% highlight lua %}
love.graphics.setColor(0.5, 0, 0)
love.graphics.circle("fill", self.x, self.y, 20)
{% endhighlight %}

This will be our player for now, in order to spawn player in the `Game.lua` in method `new` create local area:

{% highlight lua %}
self.area = Area(self)
{% endhighlight %}

and add the game object with the following code:

{% highlight lua %}
self.player = self.area:addGameObject("Player", 100, 100)
{% endhighlight %}

This will add our `Player.lua` game object into the game room at the position of x = 100, y = 100, so the final `new` method should look like this:

{% highlight lua %}
function Game:new()
    self.area = Area(self)
        
    self.player = self.area:addGameObject("Player", 100, 100)
end
{% endhighlight %}

In the `destroy` method, destroy created area:

{% highlight lua %}
self.area:destroy()
self.area = nil
{% endhighlight %}

**The mistake was made!**

!["Warning"](/assets/love-tutorials/images/Rooms_Rooms_Rooms/Warning_sticker.png "Warning") 

In function `gotoRoom` change the order of `if` condition and `current_room` variable so the variable was below condition, this way we do not destroy our room immediately when we change it from the menu to the game:

{% highlight lua %}
function gotoRoom(room_type, ...)
if current_room and current_room.destroy then 
    current_room:destroy() 
end

current_room = _G[room_type](...)    
end
{% endhighlight %}

After all this manipulation you can launch the game and in the game room see our circle player with generated id above it. If something went wrong please write me or leave a comment with the error you have.

For the end of the article, we will play with our player. If you want more than one object at a time, for example, have many enemies—in my situation bunch of players—just put `addGameObject` into `for` loop:

{% highlight lua %}
for i = 1, 5 do
    self.player = self.area:addGameObject("Player", 100 + i, 100 * i)
end
{% endhighlight %}

In this example I placed five player game objects one under another, you can launch the game and see that we now have five of them with different ids.

!["Players with different IDs"](/assets/love-tutorials/images/Rooms_Rooms_Rooms/Players.png "Players with different IDs")

You as well can provide additional options for the game object, after the third argument which is `y` add the table with as many `key - value` as you want, for example, a boolean with their well being and the name for each of them would look like this:

{% highlight lua %}
self.isAlive = true

for i = 1, 5 do
    self.player = self.area:addGameObject("Player", 100 + i, 100 * i, 
    {["isAlive"] = self.isAlive, ["Name"] = "Jonny the "..i})
end
{% endhighlight %}

Where you can access them from the `Player` or `GameObject` classes, for example, in the `new` method write:

{% highlight lua %}
print("opts are: ", opts.isAlive, opts.Name)
{% endhighlight %}

Which will prints value for the `isAlive` boolean and `Name`, but let's return to the player, add the control functional so we can move around the room, we already have `input` in the main file for our key, we need only bind key we want to use with the functional, in the `new` method of `Player.lua` file write:

{% highlight lua %}
self.speed = 50

input:bind("w", "up")
input:bind("a", "left")
input:bind("s", "down")
input:bind("d", "right")
{% endhighlight %}

and for the buttons in the `update` method:

{% highlight lua %}
if input:down("up") then
    self.y = self.y - self.speed * dt        
elseif input:down("left") then
    self.x = self.x - self.speed * dt
elseif input:down("down") then 
    self.y = self.y + self.speed * dt
elseif input:down("right") then
    self.x = self.x + self.speed * dt
end
{% endhighlight %}

This way we will have sloppy movements of our player when we press `wasd` buttons, if still have five players then all of them will move, for the end of article make our playground be in a loop like in Pac-Man game, I will make it for all game objects, you can apply this rules only to the player, to apply loop room for game object in the `GameObject` method `draw` write:

{% highlight lua %}
if self.x > love.graphics.getWidth() then 
    self.x = self.x % love.graphics.getWidth() 
end

if self.x < 0 then 
    self.x = love.graphics.getWidth() 
end

if self.y > love.graphics.getHeight() then 
    self.y = self.y % love.graphics.getHeight() 
end

if self.y < 0 then 
    self.y = love.graphics.getHeight() 
end 
{% endhighlight %}

Now if you go out of the room borders, you will appear on the opposite side.

!["The End"](/assets/love-tutorials/images/Objects_RoomsAndConway's_Rules/The_End.png "The End")

And as always files that were created, changed, modified in this part, you can find [here](https://github.com/A-Mikhail/Love-GameOfLife/tree/tut-part3). 

### TL;DR
In the tutorial we create a really basic structure for making simple games, in the structure, we have the menu and game rooms, areas that if in a few words can be described as a  manager for spawning, drawing, and destroying game objects. `GameObject` class that used as the parent class for every game object in the game, and a player with the simple controls and looped room like in the Pac-Man. 

### Before you move next
!["Leave a comment"](/assets/love-tutorials/images/Rooms_Rooms_Rooms/Leave_a_comment_banner.png "Leave a comment")

I want to thank you for reading the article, hope you find it interested to read, if not or you have something to say, please leave a comment or write directly to [me](https://mikhailadamenko.design/about). I will highly appreciate every feedback you have.

*Have a good day!*