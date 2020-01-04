---
author: Rob
categories:
- Projects
date: "2011-08-01T21:14:29Z"
guid: http://robwilliams.me/?p=215
id: 215
title: Mastermind Solver – C#.net and CUDA Versions
url: /2011/08/mastermind-solver-c-net-and-cuda-versions/
---
I haven’t posted in quite a while, but I _have_ been coding. This is the first one I am going to post about, but there’s another big one coming probably by the end of the summer.

I have become really interested in the [first algorithm](http://colorcode.laebisch.com/links/Donald.E.Knuth.pdf) proposed by none other than computer science extraordinaire Donald Knuth, with the exception of recent genetic algorithm approaches to the problem. I hacked away at implementing Knuth’s algorithm, and chose C# since I wanted to make a GUI that you could actually play as well. Aside from a few hurdles, it didn’t take long and I quite like the result. The only problem: the computer takes up to 15 minutes to solve the puzzles which exhibit the worst-case scenario (for Knuth’s algorithm, the worst case time occurs for any sequence which results in 2 white hits on the first guess).

This occurred at about the same time we needed to choose our projects for my Multicore Platforms class, where we were learning to use the [Nvidia CUDA toolkit](http://www.nvidia.com/object/cuda_home_new.html). If you are not familiar, CUDA allows you to execute general programs on the Graphics Processing Unit (GPU). While the GPU runs at a significantly lower clock speed than modern CPUs, they have a ridiculous amount of processing cores. My laptop’s meager GeForce 8600M-equivalent Quadro has 32 cores, while the more recent cards like the GeForce GTX 460 has 336 cores. You can imagine that if your code can be executed in parallel, there are serious performance benefits to be had from CUDA.

Making a Mastermind solver with CUDA seemed like a perfect fit, since Knuth’s algorithm lends itself well to parallel programming. Since CUDA programs are written in C, I had to port the algorithm over from C#, which was trivial. However, getting it optimized to run on the GPU was quite a challenge. The biggest problem was dealing with the massive memory requirements. My card only had 256MB of main memory, while the memory needed to efficiently solve the Super Mastermind variant (8 colors, 5 slots) is 21GB. I had to split the problem into iterations, which required a lot of complex design due to the architecture of CUDA. After that was taken care of, there were still other optimizations I was able to do to increase the speed further.

Anyway, once everything was said and done, the CUDA solver was able to solve the same puzzle that took 15 minutes on C# in 5.98 seconds. To be fair, the ported C version running on the Intel Core 2 Duo T9300 CPU only took 236.65 seconds, not the full 15 minutes that C# took. But this still represents a 40x speedup from the C version on the CPU to the C version on the GPU. I ran benchmarks on a faster machine to see how the solver would scale to more cores. Its Core i7 960 could solve the puzzle in 164.52 seconds, while the GPU version running on the GTX 465 took only 0.686 seconds. That’s a speedup of 240x.

I cannot stress enough how incredible programming for the GPU is. It certainly requires a different set of skills than traditional programming, but nothing that a good programmer can’t learn. The benefits are unbelievable for any application where the majority of the work is parallelizable (cf. [OpenCL](http://www.khronos.org/opencl/) is also something to look into, especially since it works on both AMD and Nvidia cards. I plan to try to learn OpenCL in the near future, especially since my desktop has a Radeon card.

I’m not going to post the CUDA code publicly for a while because I am considering publishing a paper about it, and I’m not sure what the rights are going to be. However, I’d be happy to talk to you via e-mail and probably send it to you privately.

The C# version on the other hand, click the following links to download: the [source code](/weekly/MastermindSource.zip "Mastermind Source Code ZIP") or [setup executable file](/weekly/MastermindSetup.zip "Mastermind Setup Executable").

The github repository can be found [here](https://github.com/robwil/Mastermind.Net).

![Mastermind.NET](/images/screens/mastermind.jpg) 

(P.S. This game was played by the computer, not me. My strategy is quite different. I’d be happy to talk about it via e-mail.)