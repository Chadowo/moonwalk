---
layout: post
author: Chadow
tags: [programming, games]
---

> **Note:** This project is still experimental so it may be *unstable*.

## Exporting Your Gosu Game

To get started, I'll give you some context: you've finished your game, using the [Gosu](https://www.libgosu.org/) game framework, and now you want to distribute to the players, right? Well you'll get to see it's not an easy task, since
your game is tied to the Ruby interpreter, and it's not a compiled language (e.g. C or Rust), this makes
 it so you need for your players to install Ruby in their PC, and run your code. That's really unconvenient
and I'd say almost nobody will install a programming language's interpreter to run a game, unless it's a really
nice one (case on point:Minecraft).

There's a way, though, to release your game in a nice and simple binary for your players to run. And that is to 
simply bundle the whole language runtime with your code, aka 'Freezing'. [Ocra](https://github.com/larsch/ocra) and its more active fork 
[Ocran](https://github.com/Largo/ocran) are gems that do exactly that. However, using this approach will give you some over-head and 
a bigger binary size, considering you pack the whole runtime in your binary, this is to be expected. 

## Enters MRuby

[MRuby](https://mruby.org/) is the "lightweight implementation of the Ruby language complying with part of the ISO standard. mruby can
be linked and embedded within your application.". That means that much like [Lua](https://www.lua.org/) we can embed MRuby into our own
application and use it as a scripting language, without having to install the big CRuby runtime (The de-facto Ruby implementation, and
the one you're probably using). 

Ok, so how do we use it to export a game? Well, firstly we'll need a way to use Gosu from MRuby itself, and lucky for us someone has already done
that, in the form of a mgem (equivalent of a gem for MRuby, it's not so much like a gem since MRuby has no package manager whatsoever): [mruby-gosu](https://github.com/cyberarm/mruby-gosu) by Cyberarm. 

Then what's left is integrating all of this together!

## My Wrapper

Following the previous points, I've made the [Gosu MRuby Wrapper](https://github.com/Chadowo/gosu-mruby-wrapper), as the name implies it's a simple 
wrapper for MRuby that runs with the mruby-gosu (and some others) mgems installed. Check [here](https://github.com/Chadowo/gosu-mruby-wrapper/wiki/Getting-Started)
for instruction on usage. TLDR: 

- Run it as a command line utility:  
    ```console
      gosu-mruby my_code.rb
    ```
- Use a [boot](https://github.com/Chadowo/gosu-mruby-wrapper/wiki/Getting-Started#boot-file) file:  
    ```ruby
     $: << 'my_code_files/' # Add the directory where all our code is to the load path

     require 'main' # This will kickstart the main game-loop
    ```
- [Embedding](https://github.com/Chadowo/gosu-mruby-wrapper/wiki/Fused-Mode) the source code directly into the executable. **Warning:** experimental.

To showcase something, my game [Asteritos](https://chadow.itch.io/asteritos) is packaged with this wrapper.

## Moving On

I would like to make it more stable and easier to use. I should also develop something like [Releasy](https://github.com/gosu/releasy) so the process
of exporting becomes far more easier. 

Hopefully this utility can be useful for someone.
