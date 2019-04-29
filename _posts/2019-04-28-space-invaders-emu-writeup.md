---
layout: post
title:  "Space Invaders Emulator Writeup"
categories: emulation
permalink: /space-invaders-emu-writeup/
---

It has been a while since I have writen anything on here. I got sidetracked
because I got a new job at NetApp, but hopefully I will now start dedicating
time again to these side projects. I had paused my development of a Space
Invaders emulator (at that time consisting of a somewhat correct and almost
fully implemented Intel 8080 CPU along with a monitor display), but have since
resumed and finished the project.  (Just needed to add some interrupt handling
and an IO mechanism for the CPU, an external shift register, color, and sound.)

Though it took a while, I would not consider this project to be a difficult
one. The end result is definitely satisfying. I especially like the sound which
is one of the best parts about Space Invaders. I think my sound is probably a
bit off, but it seems close enough and I'm happy with it.

After coming back from the long hiatus, I definitely started getting lazy and
just stuffed all the new features in main.c. The next emulator project I tackle
will probably be complex enough to require a bit more discipline. I also
imagine it will be worth putting in time up front to make a legitimate
debugger.

Below are some topics and my thoughts on them.

New things to learn
--------------------------

So what was new in this emulator that wasn't present in my previous Chip-8
project?  The biggest thing was definitely the CPU. The Space Invaders arcade
machine uses an Intel 8080 which weighs in at around 100 instructions.  It also
features interrupt handling and an IO mechanism to communicate to external
hardware.

The other major difference was audio! Space Invaders has a very iconic sound
that you don't want to skip when emulating the machine. Audio programming is
something I know absolutely nothing about, so it would make for a nice
challenge, but as will be discussed below, the audio implementation for Space
Invaders is not very typical. So though I got to learn a little bit about
playing audio files with SDL_Mixer, I still am pretty ignorant on how real
sound emulation works.

CPU
--------------------------

For the CPU implementation I probably went a little overboard. The original
idea was to create a base that contained the core functionality and then extend
it to handle all of the various CPU types in the 8080 family (like the Z80 or
the Sharp LR35902).  This would eliminate duplication of all the similar code
between CPUs, and also emphasize the differences between the CPUs (because
features that were unique to a particular CPU would be contained in their own
dedicated file). This was going to be helpful because another idea I originally
had was for this code to be a sort of reference implementation that would be
easy for somebody new to emulation to read. All of this led to code that is
much more verbose than what I have seen in other 8080 implementations online. I
ultimately ended up dropping these ideas in favor of just getting something
working in a relatively quick manner. Considering this now in retrospect, I
would probably slim the code down, and if I wanted to emulate a similar CPU, I
would just copy-paste and then make the appropriate tweaks. At the end of the
day, a CPU implementation isn't going to change a whole lot once it is free of
bugs.

Another design choice was to split the overall CPU logic into two main phases.
The first being the fetch/decode phase. The second being the execution phase.
This was in the hopes of easily reusing the fetch/decode phase logic for other
componenets, such as a disassembler or debugger. That being said, I never
actually wrote either of those. The means for which information would pass from
the fetch/decode phase to the execution phase was through an abstraction which
I termed an "instruction". This instruction differed from an opcode in the fact
that it was a higher level concept that represented everything to do with the
actual logic that gets executed for a given opcode. For example, it contains
the number of cycles it takes to execute, the operand types and values, etc.
This helps simplify the execution phase. One, it bundles all the necessary
information so the execution phase doesn't have to look back into the memory
again (for example to obtain the operands). Second, it separates out data that
is more or less hard coded like the cycle count. For this type of emulation,
which isn't actually simulating electrons and transistors, it makes sense to
have cycle counts all together in a simple table like structure as opposed to
littering that information all throughout the execution logic. Finally, the
instruction data structure also contains non-execution data such as the
instruction mnemonic which can be used for the aforementioned debugger and
disassembler. Overall, I like the instruction abstraction, but I did run into
snags. For example, some CPU operations actually have varying cycle counts
based on the state of the CPU at time of execution. In fact, now that I think
about it, I probably forgot to adjust the cycle count appropriately for those
particular instructions. I think the effect should be negligible though.

Testing
--------------------------

As the complexity of your emulation goes up, having some sort of automated
testing becomes absolutely essential. There were so many issues I ran into that
I would have never solved without a good suite of tests. Obviously you will
find bugs in the code you write, but what may surprise you is how common it is
to find mistakes in the documentation you read. I ended up having to cross
reference multiple sources to suss out the correct details on the system I was
emulating. 

Luckily I came across some very thorough tests for the CPU. They were also kind
of interesting because it raised a question. How does a test communicate back
results to a user? There are two methods that these particular tests use to
accomplish that. The first is by executing instructions that write a string to
memory and then subsequently adjust a specific register to point to that new
string. (The string is terminated by a special character.) The second means of
communication is by putting ASCII values into another specific register.  Both
of these methods required me to modify my CPU so that I could peek inside of it
during execution. I then wrote a test harness that loads the memory with the
given test program, executes the CPU, and monitors the CPU registers. The
harness then inspects the CPU for messages and prints them out when needed.

Sound
--------------------------

The audio used for Space Invaders is actually pretty interesting because it is
implemented using analog circuitry. What this means when it comes to emulation
though is that people simply play pre-recorded sound files instead of emulating
the hardware that makes the sounds. You'll find this even in more
professionally done emulators like MAME.

So I still learned a little bit because I had to figure out how to play sound
files using SDL_Mixer, but the task was pretty trivial compared to how it will
be like for emulating sound for a system that uses digital audio.

Misc Things
--------------------------

Another interesting implementation detail of the Space Invaders arcade machine
is how it displays the graphics. The monitor is actually black and white and
sits inside the machine pointing up. The display is then reflected using a
mirror, and the color is accomplished via overlays.

Also, to speed up gameplay, the developers created an external shift register
(which is used for moving the aliens back and forth). Without it, the CPU
can't shift data quick enough given its instruction set.

Next steps
--------------------------

I'm excited to take on a gameboy emulator next, but before that, I'm going to
take a detour and finally finish reading a book called "Crafting Interpreters".
I've been fascinated by Perl 6 lately, and am now beginning to look at Red. The
programming language stuff is fun so I want to study it more. Also, I tried to
follow along with a project called Bitwise but simply couldn't keep up. This
should be a good stepping stone to that.

Resources worth mentioning
--------------------------

There were two websites that were absolutely vital to figuring this all out.

[Computer Archeology](http://computerarcheology.com/Arcade/SpaceInvaders/)

The Computer Archeology site has tons of information on both the hardware and
software of Space Invaders. It even has a fully commented disassembly of the
ROM code!

[Superzazu's 8080 GitHub repo](https://github.com/superzazu/8080)

The GitHub repo is where I found the tests for the Intel 8080.

