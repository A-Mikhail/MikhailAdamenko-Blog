---
layout: post
title:  "Part 1 - Installing and Launching"
date:   2018-03-06 22:02:48 +0600
author: Mikhail Adamenko
---

This part of the tutorial will be prety short because of simplicity of installation. But still important for undestanding my environment on which I will be showing you other part of the tutorials.

### Installation
First go to the their [main page](https://love2d.org/) and in the Download section find the link most suitable for 

### Launching
After installing the framework, proceed with configuring our environment where we will we work.

1. Create a folder anywhere you like for your game, I call my folder `helloWorld`.
2. Create a file `main.lua` this file important to have because Lua will watch for it as a enter point to your whole game.
3. In the terminal/console go to the folder you created for game, but don't enter it, because when we debug our game through console we need to write folder where `main.lua` is exist. So it will look like this "D:/Love/"
and in the Love folder we have our `helloWorld` folder.
4. Launch our window by writing in the *main.lua* following code:

    function love.load()
        love.window.setMode(800, 600)
        love.window.setTitle("Hello, World!")
    end

5. Compile it through console just write `love helloWorld` in a few second we get the result.

You can just call Lua with empty path, it will launch the window with a name and version of installed framework.

This is it, what code on step 4 means and other aspect I will show in the next [part of the tutorial](https://please-add-link.com) 

### Before you move next
I want to thank everyone who read the tutorial, hope you find it interested to read, if not or you have an idea for future tutorials, something wasn't well explained, please, write directly to me on my [email](mikhail.adamenko@protonmail.com). I will highly appreciate every feedback you have. 

`Have a good day!`