---
layout: post
title:  "Part 1 - Installing and Launching"
date:   2018-03-06 22:02:48 +0600
author: Mikhail Adamenko
---

This part of the tutorial will be pretty short because of simplicity of installation. But still important for undestanding an environment of which I will be working.

## Installation
First, go to the [main page](https://love2d.org/) and in the Download section find the link suitable for your case.

If you as I am use Ubuntu or lunix based on it, than you can use [ppa](https://launchpad.net/~bartbes/+archive/ubuntu/love-stable), in the terminal write commands below:

    sudo add-apt-repository ppa:bartbes/love-stable
    sudo apt-get update
    sudo apt-get install love

To check is everything installed you can launch default Love application by writing in the terminal:

    love

Result of the command should look like this:

![Default window with version of the framework](/assets/love-tutorials/images/InstallingAndLaunching/Window-after-love-command.png "Default window of the framework")

Or if you want just check a version:

    love --version

This is it, if you have window above than you can safely move to the next part of the tutorial. If you have problem  please let me know so we can fix the problem and update the tutorial to help readers.

## Launching
Now when we have installed framework proceed with configuring our environment where we will be working.

You can create whenever you want and use any names you want for project names except ones which will be requiered for correct working. Required items will be mentioned!

1. Create and name a folder anywhere you like for your game, I created folder `love-introduction` in the `/home/mikhail` directory.
2. Create inside the folded file `main.lua` 

    *Be aware! This file important to have, because Lua will watch for it as an enter point to your game.*

3. In the terminal/console go to the folder you created for game, but __don't enter it__, because when we debug our game through console we need to write name of the folder where `main.lua` is located. 

In my case path will be:
    
    cd /home/mikhail


Alternatively, you can point to the folder from any position in the terminal:

    love /home/mikhail/love-introduction

in this situation you need to specify folder where `main.lua` is located.

Now after setting everything up, let's define our window so we know that everything works properly. For now in the `main.lua` write following code:

    function love.load()
        love.window.setMode(800, 600)
        love.window.setTitle("Hello, World!")
    end

Little explanation of the code above: 
    
    function love.load() ... end

Loads object only once at the launch of the application.

    love.window.setMode() -- configure displaying the window
    love.window.setTitle()

are object of the `window` in which we defined width as 800, hight as 600, and set title of the window as Hello, World!

All atributes of [window.setMode](https://love2d.org/wiki/love.window.setMode).

Now, compile it, in terminal write `love` command with followin folder where main.lua` file is located:
 
    love 
 
or if you define whole path in my case it will look this:

    love /home/mikhail/love-introduction

Just in a few second we get the result:

![Setted up window](/assets/love-tutorials/images/InstallingAndLaunching/Configured-window.png "Setted up window")

Which is boring black screen with the title we wrote.

For now this is it, we installed the framework, created environment, configured name and size of the window, and launched it! Next we will move deeple into the structure of the framework in the next [part of the tutorial](https://please-add-link.com).

### Before you move next
I want to thank you for reading this article, hope you find it interested to read, if not or you have an ideas or something wasn’t well explained, please write to me on my email. I will highly appreciate every feedback you have.

*Have a good day!*