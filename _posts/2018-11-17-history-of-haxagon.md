---
layout: post
title:  "The history about Haxagon"
date:   2018-11-17 12:00:00 +0200
categories: home
excerpt_separator:  <!--more-->
---

Before I begin what Haxagon is, I need to give a quick introduction to SMBX 1.4 (Super Mario Bros. X). To those, who already know what SMBX 1.4 is can skip this section.

## What is SMBX?

SMBX (1.0 to 1.3) is a fan game made by Redigit. It is a mixup from various Super Mario games and even includes assets from Metroid or Zelda. It has gained a lot of popularity over the time but development has been ceased in 2011 so that SMBX 1.3 was the last version which was officially released. Over the time SMBX has been improved by user patches, most notably by LunaLua/LunaDLL. In 2015 a user named "5438A38A" posted a screenshot with an own version of SMBX named "SMBX 1.4" on the Chinese forum board [baidu](https://tieba.baidu.com/smbx). A lot of user speculated that this was fake. However in this screenshot there is a snippets of code included, which looked genuine. A few months later "SMBX 1.4" was relased on the baidu forum board. It didn't take long until players outside of China noticed about the existence of this game. The core of SMBX 1.4 is pretty good given that it was written from scratch. It is compatible with SMBX 1.3 but has complete different physics.

## The minigames

Now SMBX 1.4 was very popular at this time, especially at the chinese community. But players and designers noticed something in the editor. In the config file of the game there was config entry named `DisableHiddenFeatures=true`. Setting this to false seemed to do nothing at first, but after a while new buttons and strange text boxes appeared. Interacting with those triggered something that nobody really expected: Minigames. It includes [various challenges](https://wohlsoft.ru/forum/viewtopic.php?f=58&t=778) like math questions, logic games, ect... If you successfully finished one of these challenges you were rewarded with more features you could use in the editor.

But there were several problems:

* Some of the minigames are really hard to solve.
* Some of the features unlocked were important as they should be available in the first place.

## Haxagon

While I was developing [LunaLua](https://wohlsoft.ru/LunaLua/) I decided to use my knowledge in hacking also for SMBX 1.4. I started up IDA Pro and did some data crunching. I opened up a closed group for people who I trusted. I didn't want to let everyone know about it especially because it 5438A38A was actively working on it. It didn't take long until I've found the function which triggers the unlock.

So now I had to inject my own code now that I knew which function was important. Now there are several way in doing so. The easiest way would be DLL Injection. But luckily SMBX 1.4 uses a open-source DLL called SDL_Mixer which is loaded into memory. So basically I only had to add my code to this DLL. And this is how the first version of Haxagon was born. The name "Haxagon" is related to "Hax" which is a shorted version of "Hacks". Basically a program which hacks into SMBX 1.4.

### First version

The first version of Haxagon was very simple. While the game was loading, a console would pop up with the request whether Haxagon should be activated. If yes, then you could select the haxagon tasks you want to activate. The first haxagon task was unlocking the features by just calling the native function of the SMBX binary. I've released this version to a very small group of people I could trust and they appreciated it. After that I decided to investigate even more things about SMBX 1.4. What about all those TEA files? What about resource.tea? Are there more functions to TEA-Script. We tested, we tried, we had a lot of fun. The first version of haxagon was even able to decrypt encrypted level files.

![The first version](/assets/2018-11-17-haxagon/haxagon-0.png)

![Haxagon programs](/assets/2018-11-17-haxagon/haxagon-1.png)

### Haxagon Remastered

There is a huge problem with the first version of haxagon: It isn't user-friendly. A well-designed GUI would be better, at least for haxagon tasks which required a user inputs. But C++ isn't that good with GUIs. So I needed another language and I selected C#. C# has winforms which provides an easy way to create native GUIs. But with C# I would lose the possibility to use SDL_Mixer to hack me into the program without a hassle. So I needed an equally good solution for C# and luckily I found EasyHooks. With EasyHooks I can hack me in at run-time and do the necessary patches. And so was [Haxagon Remarstered](https://wohlsoft.ru/forum/viewtopic.php?f=58&t=2610) born, the one I eventually released to the public.

### The Reaction

I've hold off Haxagon from the public for about 2 years because SMBX 1.4 was in active development. I did set a rule for me: If SMBX 1.4 wouldn't receive a major update longer than one year, then I would release it to the public. And so I did that eventually. Now it can be debated whether it was right or wrong, but I think holding off features isn't a right way to go. 5438A38A decided to "pause" development. Nobody knows what the future will be for SMBX 1.4 but let's wait and see...

## Some Facts

* Resource.tea contains "hardcoded" assets like images or minigame data
* Shell.tea is the encrypted version of the standalone smbx-game (without the editor)
* 5438A38A has banned several QQ-Users from the game by searching the system for the QQ-Client and reading the QQ-Number (only applies to Chinese computers)
* SMBX 1.4 includes several anti-tamper, anti-cheat systems, including:
  * Packed EXE: Compresses the EXE to prevent reverse-engineering
  * Runtime ASM: The game generates runtime x86 Assembly for encryption and compressing
  * MD5 Validation: The game validates TEA-Files with hardcoded hashes
  * Anti-Debugger: The game closes when a debugger is attached
  * Unpacked EXE check: The game closes if it detects that the EXE file was unpacked
  * Cheat-File-Checks: The game closes when it detects cheat engine or similar cheating systems
  * Time-Checks: The game closes if specific functions don't complete in a certain time as it thinks that a function was intercepted.
