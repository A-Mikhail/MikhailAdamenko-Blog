---
layout: post
title:  "Part 1: Install, Configure, Launch"
date:   2018-03-06 22:02:48 +0600
author: Mikhail Adamenko
image: "/assets/love-tutorials/images/InstallingAndLaunching/Love-11.0.png "
---
In the [previous part](https://mikhailadamenko.design/love/2018-03-06-Into-the-love.html) of the tutorial, I described what framework LÖVE is. In this part, we will go through the installation, configuration, and launching our first window of nothingness. Also, I will mention two important files to have `main.lua` and less important `conf.lua`, as well as standard functions `load`, `update` and `draw`.

The configuration for the framework I will show for Linux version, other parts of the tutorial will be work in every system as well, but I should say that installing process is simple enough.

### Installation
The first what we need to do is ... right! Download the framework, to do so, go to the [main page](https://love2d.org/) and in the Download section find the link suitable for you, at the moment of writing the article the last version of the framework is `11.0`.

If you, as I use a Debian-based Linux than you can use [PPA](https://launchpad.net/~bartbes/+archive/ubuntu/love-stable), to get the latest version of the framework, and in the future receive updates through the update system. For PPA usage you need in terminal write commands below:

    sudo add-apt-repository ppa:bartbes/love-stable
    sudo apt-get update
    sudo apt-get install love

The commands above will install the LÖVE framework into your system globally, which is everything you need to do for writing a game.

To check if everything properly installed, you can launch default LÖVE application by writing in the terminal:

    love

The result of the command should look like this:

![Default window with version of the framework](/assets/love-tutorials/images/InstallingAndLaunching/Love-11.0.png "Default window of the framework")

Which is the default application of the framework, each minor version has its unique animation as well as the name, for version 11.0 it is `Mysterious Mysteries`.

If you want just to check a version then write the command:

    love --version

The command displays current version in the terminal.

If you have the window above then installation is done, you can safely move to the next part of the tutorial.

### Launching

Now, when we have the framework installed next what we will do is configuring our environment where we will be working for the rest of the tutorials.

Naming for all the objects including files, folders, functions, classes in this tutorial and the future will be provided as I use them, in my environment, you can safely use your own unless it's mentioned to use provided.

1. Create and name a folder anywhere you like for your game, I created a folder `GOF` (Game of Life) in the home directory inside folder love: `~/love/`.

2. Inside the folder create a file `main.lua` 

    *Be aware! This file is important to have, because LÖVE will be needed it, as an entry point to your game.*

3. Create second file `conf.lua` which will contain configurations for the game. 

    The file is not required. The framework has its settings which will be applied if no configurations aren't given. 


#### main.lua
The file `main.lua` is the main file from which our game started, the file can have any functions and variables you like, but it's also important to have these 3 functions `load`, `update` and `draw`, although the function have already defined in the source of the framework, so you can launch an application right now, if all you want is an "Untitled" black screen.

In order to launch an application, in the terminal write `love` followed by the folder where file `main.lua` is placed. For me it will be:

    love GOF

If you want to launch an application from every place, then include the full path to the folder, which in my situation:

    love ~/love/GOF

here second `love` is the name of the folder.

#### load
After we launched our empty application, we can start to set it up and to do so we start with the first important [load](https://love2d.org/wiki/love.load) the syntax of the function:

    function love.load()
        -- the body of the function
        -- btw, this is the comment line
    end

What "load" function do? It initialized objects inside it only once at the start of the program. Thus, the function is perfectly fit for defining variables both local and global, images, fonts, shaders, and other objects.

In order to show the function at work, we give our window a name, by writing in the body method [window.setTitle](https://love2d.org/wiki/love.window.setTitle):
    
    love.window.setTitle("Game of Life!")

This will set name for the window to `Game of Life!` you can launch and see the result.

Next function after `load` is `update` with the following definition

    function love.update(dt)
        -- body of the function
        -- function update all its
        -- content every frame or delta time (dt)
    end
    
#### update
The [update](https://love2d.org/wiki/love.update) function useful when you want to do some action every frame, for example, increase the counter by 1 every second. To do so, you need to initialize a variable that holds value outside the function in the `load` or out of all functions.

    dtotal = 0   -- this keeps track of how much time has passed

In the Lua all variable by default is global.

Then in the function keep adding [delta time](https://en.wikipedia.org/wiki/Delta_timing) (dt) to the variable `dtotal` 

    dtotal = dtotal + dt

which is the small value around 0.01, you can check it by yourself just printing the value of `dt` in the function to the terminal

    function love.update(dt)
        print("value of the dt: ", dt)
    end

After the increase of our variable, we need to check when the value will be equal to 1 second:

    if dtotal >= 1 then
        -- return to the original state
        -- if we want to do it in the loop
        dtotal = dtotal - 1

        -- do some action every second
        print("a second has passed")
    end

I should mention that this is not the best implementation of the timer, because it depends on delta-time which can be rewritten and cause total chaos, but as the example, it does its job.

#### draw
The third function is the [draw](https://love2d.org/wiki/love.draw), used in the framework to draw on the screen every frame. objects like [Images](https://love2d.org/wiki/Image), [Canvases](https://love2d.org/wiki/Canvas), [Particles](https://love2d.org/wiki/ParticleSystem), [Meshes](https://love2d.org/wiki/Mesh), [Texts](https://love2d.org/wiki/Text) should be in this function, outside of the draw function, they will not be displayed on the screen, although still work!

But if you want to have the drawable object be outside the function, you can put them inside a [Canvas](https://love2d.org/wiki/Canvas) and call from the `draw` function, which I will explain in the future tutorials.

For now, I will show how works the `draw` function on the example of FPS counter, which is useful in the development process to know how well games go. So, let's write our FPS counter by using standard framework counter function.

First, we need to change the color of the displayed text from the default black to white so we can see it on the screen. All color changes happened only with this one method [graphics.setColor()](https://love2d.org/wiki/love.graphics.setColor), inside the `draw` function write:

    love.graphics.setColor(1, 1, 1, 1)

This will set all preceding graphics element to be a white color. The method [setColor()](https://love2d.org/wiki/LÖVE.graphics.setColor) used range from 0 to 1 in the following order (red, green, blue, alpha), so if you want to have colors other than black (0) or white (1) you can get simple RGB value and divide each color by 255, to have exact color.

Next, after we change our color, let's print the counter to the screen by using method [graphics.print()](https://love2d.org/wiki/love.graphics.print) with [timer.getFPS()](https://love2d.org/wiki/love.timer.getFPS) which return current fps in [number](https://love2d.org/wiki/number) value.

    love.graphics.print("Current FPS: "..tostring(love.timer.getFPS()), 10, 10)

The 1st argument in `graphics.print()` method is a string `"Current FPS: "..tostring(love.timer.getFPS())` two dots in the Lua connect the first line with the following in our case converted from number to string method `getFPS()`

The 2nd and 3rd arguments are position x, y on the screen in px.

In result, we get our FPS counter in the top-left corner.

![FPS counter](/assets/love-tutorials/images/InstallingAndLaunching/FPS-counter.png "FPS counter")

#### conf.lua
Briefly mention configuration file for the framework, the default setting of the file you can find [here](https://LÖVE2d.org/wiki/Config_Files). In the file, you can change the width and height of the window as well as the name, and many others parameters. The width and height can be changed later in the game, so in the file, we set default setting for the app.

### Outro
For now, this is it, we installed the framework, created the environment and launched the application! Next, we will start to build our application for making "Conway's Game of Life", explanation of it and a few useful Lua libraries which will help us in the developing process.

[Main.lua](https://github.com/A-Mikhail/Love-GameOfLife/blob/tut-part1/main.lua) and [Conf.lua](https://github.com/A-Mikhail/Love-GameOfLife/blob/tut-part1/conf.lua) written in the tutorial.

### TL;DR
First, we download and install the framework, then set up an environment, and at the end, simple explanation of the three main functions load, update and draw.

### Before you move next
I want to thank you for reading the article, hope you find it interested to read, if not or you have something to say, please leave a comment or write directly to [me](https://mikhailadamenko.design/about). I will highly appreciate every feedback you have.

*Have a good day!*