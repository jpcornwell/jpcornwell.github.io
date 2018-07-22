---
layout: post
title:  "Chip-8 Writeup"
categories: emulation
permalink: /chip-8-writeup/
---

TODO: Once I have jekyll working again, add some screenshots


So I just finished up two implementations of Chip-8. One is in Python using
PyGame. The other is in C using SDL2. They are fully fleshed out with the
exception of sound, which I decided to skip because it only consists of a
single pitch beep.

Python Implementation
---------------------

The Python implementation was meant primarily to reacquaint myself with Chip-8
and emulation topics. I did learn a couple of new things though. 

First, I learned that hard coding the direct path to your Python executable in
a shebang line is not a very good way to do things. Instead it is preferred to
code it to the 'env' executable, and have that find Python for you. This not
only is less fragile, but makes dealing with virtual environments much easier.
So for example use '#!/usr/bin/env python' instead of '#!/usr/bin/python'.

Second, I learned about pipenv. (By the way, pipenv's use of virtual
environments is why I came across the first item about the preferred shebang.)
All the different tools and work flows for handling third party packages and
keeping them separate among your different projects is kind of crazy and a bit
intimidating for a new comer. You've got pyenv, virtualenv, pyenv-virtualenv,
virtualenvwrapper, pipenv, pyvenv, and venv. (There are probably others I've
missed.) In fact, at first it seems to go very strongly against Python's 'One
way to do things' mantra. Once you dedicate some time with it though, you'll
find that it isn't too bad actually. (I'll probably write a post on this, but
the thing to remember is that most everything revolves around virtualenv, and
the things that don't either have plugins to work with virtualenv or they were
meant as replacements for virtualenv but didn't gain traction.)

C Implementation
----------------

The motivation behind the C implementation was for me to regain familiarity
with C and to learn SDL.

I found that the most recommended resource for learning SDL was a set of
tutorials from Lazy Foo Productions and they do indeed seem to be the best
freely available tutorials out there. The approach they take is to simply
present code and then explain what is going on, introducing new features one at
a time.

This example driven method is probably the way most people like to learn, but
for me, I do much better when given the concepts first. For example, I want to
understand what surfaces and renderers actually are before I start pushing
various shapes to them.

All in all, it's not that big of a deal, because SDL is pretty small, but it
would still be nice if something like that existed for those who are brand new
to the library. Perhaps one day I could try my hand at writing this kind of
tutorial.

Timings
-------

The biggest take away from this project was figuring out how to handle timing
when emulating a system. At first, I thought I would simply execute a cycle per
every Xth of a second. However, this becomes impractical very quickly. 

Using SDL, the most precise unit of time you have to work with is the
millisecond.  This works fine for typical 'frames per second' type timings in a
video game, but it doesn't work at all for hardware timing. The Chip-8 running
at a mere 1KHz would push us to the very limit of one cycle per one
millisecond.

Therefore, you either need to use a more precise measurement or start running
multiple cycles per unit of time. For example, you could have an abstraction
called a tick, and then have 60 ticks per second, and X cycles per tick. This
way your throttling can be done at the tick level. Using this approach also
makes it easy to adjust speed by modifying the number of cycles per tick, and
it allows you to sync your ticks up to the host's display system.

Next Project
------------

So my next emulator project will be the Space Invaders arcade machine. It will
be slightly more complex and include higher resolution graphics, multi-tone
audio, hardware interrupts, a full fledged CPU, and communication between
components via memory mappings.

The CPU will be an Intel 8080 which is good because the implementation can be
reused on the following emulator project, a gameboy!

