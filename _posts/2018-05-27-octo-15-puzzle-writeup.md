---
layout: post
title:  "Octo 15 Puzzle Writeup"
categories: game chip8 writeup
permalink: /octo-15-puzzle-writeup/
---

So it took a while, but I am finally now putting to paper my thoughts on my
last Octo project which was an implementation of the sliding 15 puzzle.

The program turned out alright, but this will serve as a lesson to better plan
my future projects. The main thing is I am not super into the 15 puzzle. It
would have been better to do a project that I would actually have fun playing
afterwards. My thinking had been that it would be a good stepping stone to a
Connect 4 game, but I lost motivation and it sort of just dragged on. 

In fact, I feel I got rather sidetracked working on the Chip-8 games. I had it
in my head that I was going to make really awesome Chip-8 games (and maybe in
the future I can still do that) but my original goal was simply to get a better
understanding of Chip-8 to help with writing my own Chip-8 implementation.

Ultimately, if I want to delve into low level game programming, I'd probably be
better off doing that on game boy or NES. Chip-8 is extremely limited. In fact,
I found that John Earnest's online Octo IDE that I had been using, was by
default running programs at 20 cycles/frame, but my research tells me that
original hardware would be closer to a running rate of 7 cycles/frame. In other
words, as constrained as I felt, the Octo IDE was still giving me quite a bit
of leeway.

On the actual programming side of things, it definitely pays off to plan out an
Octo program. Since I had already done the snake game, I figured I would just
start cranking out some code. The result was frequently needing to track down
bugs where I had overwritten a value in a register that was used elsewhere in
the program. Taking a month long break in the middle of the project didn't help
either. The process would have been a little smoother if I had taken the time
to map out the registers and their uses.

I think I will now take a break from Chip-8 games and write up another Chip-8
interpreter. I'd like to do it in C, which I need to brush up on, and along the
way I can also learn a bit of SDL. This should then set me up to be in a
position where I can tackle a Space Invaders emulator next.

