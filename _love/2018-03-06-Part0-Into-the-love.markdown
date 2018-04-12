---
layout: post
title:  "LÖVE framework on the example of Conway's Game of Life"
date:   2018-03-06 11:55:14 +0600
author: Mikhail Adamenko
image: /assets/love-tutorials/images/Introduction/Introduction.png
---

### Narration
Frameworks. Because of them, I stopped to work in a web-development sphere for almost two years. I was overwhelmed by them, each day I saw a new, shiny, modern tool with the promise to fix problems of a predecessor. Some of them were the solution to problems, some were the problem. 

Most of the time I tried to keep up with popular frameworks. Unfortunately for me, I can’t do it long enough and move to the relatively steady C-based languages, where everything is calm.

Now, After some time, I want to discover frameworks, but instead of learning which one is better. I decided to try as many frameworks, tools, programming languages, as possible, by accomplishing small projects from scratch to somewhat finished product and based on it, write an experience that I have in form of tutorials. With hope that it helps someone.

First series I want to begin with this one. The [`LÖVE`](https://love2d.org).

### What is LÖVE?
The main page described it as an awesome framework for creating 2D-games. The framework used Lua language for interacting with lower level architecture like: SDL2, OpenAL, FreeType PhysicsFS, mpg123 and [others](https://bitbucket.org/rude/love/overview), available technologies in usage directly is OpenGL - more precisely GLSL for writing shaders, others are available throughout Lua. 

The framework and the language are pretty simple to learn, so a big portion of your time will be well spend on prototypes, games, or ideas that you may have. 

The framework has a large [community](https://love2d.org/forums/), different tools, and [libraries](https://github.com/love2d-community/awesome-love2d) written specially for usage with the `LÖVE`. The legal side of the framework allows you to make an open-source project as well as commercials.

### Why to use LÖVE?
`LÖVE` can be perfectly fit for:

1. __Prototyping__ --- The Lua language, well-written wiki and a minimally required effort path from the installation to a simple application in just a few minutes make it great for prototypes that will work, and can be transferred on other technologies or developed further.

2. __Lightweight__ ---  Consistency of relatively small components make the `LÖVE` size small in the finished product. The __.love__ extension, made an application even smaller because of separation of the framework and an application to different parts. But, even with the `LÖVE` dependencies linked together it still has a small size, which is good if you have size limits for some of your devices you develop, or just liked when finished product hasn't unused libraries.

3. __Portfolio__ --- It's always great to show something you made in your portfolio even if it crashes every five minutes or ended as a prototype of an early-stage product, experience earned from the small project will always be useful.

4. __Experience__ --- Finally, an experience you can get from the developing process, usage of the small frameworks will help you to better understand your own capabilities, adjust expectation and prepare for bigger projects. With a small start, you will waste much less time, if you will stuck in the process of developing process, because of lacking some skills, knowledge or understanding that a chosen pack of technologies wasn't right for you. 

### Who already used LÖVE?
You can find more on the [main page](https://love2d.org/), but if you don't want to, here is a little recap:

1. `Mr. Rescue` by `Tangram Games`

![Mr.Rescue Game screenshot](/assets/love-tutorials/images/Introduction/mrrescue.png "Mr. Rescue Game screenshot")

"An arcade styled 2d action game centered around evacuating civilians from burning buildings.

The game features fast paced fire extinguishing action, intense boss battles, a catchy soundtrack and lots of throwing people around in pseudo-randomly generated buildings."

__Game Page__: <http://tangramgames.dk/games/mrrescue/>
        
__Source code__: <https://github.com/SimonLarsen/mrrescue>

2. `Move or Die` by `Those Awesome Guys`

![Move or Die screenshot](/assets/love-tutorials/images/Introduction/moveordie.png "Move or Die screenshot")

"An absurdly fast-paced, 4-player local and online party game where the mechanics change every 20 seconds. The very definition of a friendship-ruining game, Move or Die is easy to pick up but hard to put down."

__Game Page__: <http://www.moveordiegame.com/>

__Developes Page__: <https://thoseawesomeguys.com/>

3. `Oh My Giraffe` by `Nico Prins`

![Oh My Giraffe screenshot](/assets/love-tutorials/images/Introduction/ohmygiraffe.png "Oh My Giraffe screenshot")

"A game about a giraffe eating fruit while being chased by lions."

__Game Page__: <http://www.ohmygiraffe.com/>

__Developer Page__: <https://www.nicoprins.com/>

And many more big and small games build by using LÖVE. One of which is helped me to make mine - `BYTEPATH` https://github.com/SSYGEN/blog be sure to check it. `SSYGEN` wrote an incredible blog of how he makes the game. 

### Background
For the `LÖVE` and `Lua` you won't need much to know, but some useful knowledge to have is:

1. Understanding of the basic programming - loops, arrays, variables, functions, OOP, etc.

2. Work with the Window or Unix console. Simple commands like --- install or uninstall, creating, deleting. Nothing special is needed to know.

3. Some familiarity with an IDE of your choice, for syntax highlight, git integration, debugging and many more which will certainly help you in the moment of development.

### Let's Try It Out
Now, when you have some knowledge about the framework, we can take our first step into the framework in the following articles.

### Lua
If you interested in learning syntax of Lua before moving next there has one great article --- [Learn X in Y minutes](https://learnxinyminutes.com/docs/lua/)

### TL;DR
Tired of many frameworks used on half, the author decided to dig into different frameworks to understand their possibilities. The first in the series is --- Lua-based `LÖVE` framework which is easy to learn, easy to start, easy to fall in `LÖVE` with. Great for prototyping, learning the Lua, and for a portfolio. 

### Before you move next
I want to thank you for reading the article, hope you find it interested to read, if not or you have something to say, please leave a comment or write directly to me. I will highly appreciate every feedback you have.

*Have a good day!*