---
layout: post
title:  "Part 2 - Conway's Game of Life - Beginning"
date:   2018-03-10 13:10:03 +0600
author: Mikhail Adamenko
---
### Foreword
This is a part 2 in the series of discovering `LÖVE` framework. Previous parts consist of describing what the framework `LÖVE` is and installing and configuring an environment. Previous tutorials you can find [here](link-to-medium-articles) or in my very simple [website](https://mikhailadamenko.design).

From now on, to the end of `LÖVE` tutorials I will be showing the framework by creating Conway's Game of Life piece by piece.

### Conway's Game of Life
Known as well as __Life__ a simple simulation game of life based on simple rules. The player in some iterations of the game can set a first generation of cells, but mostly they are created randomly on a base of four rules:

1. Any live cell with fewer than two live neighbours dies, as if caused by underpopulation.
2. Any live cell with two or three live neighbours lives on the next generation.
3. Any live cell with more than three live neighbours dies, as if by overpopulation.
4. Any dead cell with exacly three live neighbours become a live cell, as if by reproduction.

Take a representation of a live cell as __1__ and dead as __0__

![Live and Dead cell](/assets/love-tutorials/images/MakeGameOfLife/Live-Dead-Cell.png "Live and Dead cell")

Other part of the game will be explained as the tutorials continues.

### Back to LÖVE
