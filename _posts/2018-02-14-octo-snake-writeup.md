---
layout: post
title:  "Octo Snake Writeup"
categories: game chip8 writeup
permalink: /octo-snake-writeup/
---

I came across CHIP8 while investigating how to get started on emulator
programming.  Technically it is an interprter but it uses a virtual machine
that has many similarities to older video game consoles which makes it a
perfect place to start learning the basics of emulation.

So to get started I wrote my first CHIP8 game using a language called Octo.
This was my first real foray into game making, and though it is simply a snake
clone, I am pretty happy with how it turned out. The main goal was to throw
something together relatively quickly to get a feel for CHIP8 before I started
making emulators for it.

Before I started coding this game up, I took a brief look at a few CHIP8 snake
games and saw that one of them had some pretty bad flickering. I didn't look at
the code because I wanted to figure everything out myself, but I figured that
it was clearing the screen and redrawing the entire snake every time it moved.
Because CHIP8 doesn't have any sort of timing mechanism for when it draws to
the screen, it can cause this kind of flicker if you have to many instructions
between when the screen is cleared and when the screen is drawn to.  Therefore
to reduce flicker, you reduce the amount of processing time between clearing
and drawing.  I decided to do this by leaving the body of the snake alone, and
only updating the head and tail. This is accomplished through a queue like
structure that keeps track of the player's movements so that the program knows
where the tail goes during each tick of the clock.

I originally had written the game so that the speed could be adjusted
dynamically. As the player's score got higher, the pace of the game would
increase. This turned out not to be that much fun though, so I ditched the
idea.

Instead, I decided that what really makes a snake game fun is being in a tight
area that forces you to frantically turn to avoid collisions. So in order to
provide for this, I went ahead and created game boundaries to limit the
player's movements. (I could probably go much further with this idea, but
again, the whole point of this was just to get something working. Maybe in the
future I will play more with this concept.)

One issue I ran into, however, was getting the controls to feel right. It took
me a while to figure it out, and I feel my solution is probably suboptimal, but
it works. At first I would periodically call a function for checking user
input, and that function would simply check to see which keys were being held
down, and calculate the desired direction accordingly. What I found though, was
that I've grown accustomed to games that react not to a key being held down,
but to when a key is first pressed. Then if the key is held for longer, the
game ignores it. The reason this is important, is because if you are trying to
make a hairpin turn, say you are going right, and you press up and left as fast
as possible, most of the time, you will actually press left before you release
up.  This was causing issues for my game and making the snake move in ways it
felt like it shouldn't.

To fix this, I added a whole lot of code to keep track of key state, so that
when I called the user input function, I would know if a key was being pressed
at that moment, if it was being held down from a previous call, or if it had
just been released. This gave me a lot more control on how I handled the input.
It also slowed the game down a lot, but I think the slower speed actually ended
up being better overall for the gameplay.

Looking back through the code after writing the game, I see there is room for
improvement. The biggest thing I could probably do would be to reorganize the
code to use functions that actually take in arguments and return values. I
didn't pursue it this time, because the game is so simlple that you can get
away without it. I would be interested to learn the best way to do this though,
so that you don't accidentally clobber register values. For example, how do you
keep track of arguments in each function if you have multiple calls on the
stack. (As far as I know, CHIP8 doesn't keep arguments in its call stack.) One
example of where functions would have been handy is when the score is drawn
onto the screen.  This is duplicated in 3 different locations and could easily
be reduced to a single function that takes in arguments for the x and y
coordinates.

My next step for CHIP8 is going to be analyzing the code from other CHIP8
games.  Some topics I am particularly interested in include managing data
structures, handling user input, rendering text, and creating animations. All
of these would be useful for the next few games I have in mind. 

