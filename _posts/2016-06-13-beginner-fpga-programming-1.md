# Beginner FPGA programming using open source tools #1: Introduction

In this blog series I will document my adventure in learning how to program iCE40 FPGAs using only open source tools. I have no prior experience using Verilog or any of the software involved so the aim is a tutorial suitable for beginners.

Field programmable gate arrays (FPGAs) are reconfigurable hardware. When you program an FPGA you define hardware logic blocks and connections between them allowing for high speed parallel execution of digital logic designs.

![FPGA](/content/images/2016/06/fpga.png)
<a style="font-size:1rem" href=http://www.vision.caltech.edu/CNS248/Fpga/fpga1a.gif>image credit: W.T.Freeman</a>

FPGAs are how new processor designs are prototyped and they have niche applications where high speed and parallel processing and a large number of IO (inputs and outputs) are a distinct advantage.

Their major downsides are their cost and the complexity and license restrictions of the vendor specific toolchains. Most of the time you don't want to use an FPGA, but when you do, oh boy are you in for a treat.

![Xilinx ISE](/content/images/2016/06/xilinx-1.jpg)
<a style="font-size:1rem" href=https://www.youtube.com/watch?v=VIHSe2dsFvE>image credit: Paolo Santinelli</a>

Vendors provide heavy bloated IDEs that are gigabytes big, slow and seem to include everything but the kitchen sink. These are the only way to program their devices. They restrict you in what features you are allowed to use and even which of their devices you are allowed to program depending on what licensing fees you pay.

Even more annoyingly it takes considerable time investment to learn the work flows and quirks of a toolchain that will tie you to a certain product line of a certain vendor.

The reason vendors get away with this is that they keep a tight grasp on their bitstream formats, an encrypted communication protocol that allows you to re-program and reconfigure the FPGA.

I last used FPGAs in University and, though I was fascinated by the technology, it was not a pleasant experience. Since then, while mostly programming microcontrollers and embedded Linux applications, I have become accustomed to a work-flow involving fast, single-purpose programs executed from the bash command-line. I use `vim`, `gcc` and `make` in my day to day and I rely on the shell's repeatability (Ctrl-R is your friend!), configurability and scriptability as a productivity boon.


I had pretty much written off FPGA work as "not my thing" until about a year ago when [an announcement](http://youtu.be/u1ZHcSNDQMM) was made that some friendly hackers (Clifford Wolf and Mathias Lasser) had reverse engineered the Lattice iCE40 bitstream and one is now able to program them completely from the command-line using only open source tools.


![Verilog with Make](/content/images/2016/06/make.png)

[Documentation](http://www.clifford.at/icestorm/) and [presentations](https://www.youtube.com/watch?v=9rYiGDDUIzg) followed and one of the most interesting revelations was that these efforts could enable on-the-fly reconfiguration of hardware from a software program. This is a new way to do computing that could be a revolutionary solution for some problems.

The easiest way to come to grasps with new way of thinking is to imagine an add-on board for a Raspberry Pi (a shield or hat, if you will) and the software running on the embedded Linux platform could re-configure its attached hardware when it needed to to accelerate a certain computation or access the vast inputs and outputs (144 in the case of the iCE40s) available from the FPGA.

Plans for exactly this are already under way. Clifford Wolf himself has been working on the [Ico Board](http://icoboard.org) (pictured) which is currently in a beta release phase and the [CAT board](https://hackaday.io/project/7982-cat-board) is another notable project attempting to provide something similar.

![Ico Board](/content/images/2016/06/ico-board_cropped.jpg)
<a style="font-size:1rem" href=http://icoboard.org/>image credit: OnSite Broadcast eU</a>

The excitement around these advancements is tangible with comparisons to the beginnings of the GNU Compiler Collection being thrown around. It really remains to be seen what the true value of an open source reconfigurable toolchain is and what a community of free acting hackers will build with this.

This is a venture strictly for fun and not for profit as the currently the marketability of the skills of using this particular workflow is questionable at best. Join me in exploration of this frontier of open source.

Let's set up our tools in [part 2](/beginner-fpga-series-2/).
