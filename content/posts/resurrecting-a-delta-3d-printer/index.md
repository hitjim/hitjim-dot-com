---
title: "Resurrecting a Delta 3D Printer"
subtitle: "If you give a Jim a Kossel..."
date: 2020-08-17T00:50:55-05:00
lastmod: 2020-08-17T00:50:55-05:00
draft: false
author: ""
authorLink: ""
description: ""

tags: []
categories: []

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: "20200808_kossel-preview.jpg"

toc:
  enable: true
math:
  enable: false
lightgallery: true
license: ""
---

## "You can have it..."

***"Do whatever you want with it, as long as you don't bring it back."***

This adventure began as a transaction with atypical terms. Not only did they generously favor me, but they were exceedingly clear and amicable for both parties.

Now, like many things that don't cost you any money, this thing has been expensive in challenging and satisfying ways.

I'd recently caught up with an old coworker via video chat for the first time in months. I shared a story about wanting to finally get a 3D printer, but ultimately chickening out. I figured I'd probably use a drill press or a belt+disc sander before I ever made good use of a 3D printer. Well, his solution was to send all 3 my way, as he had bigger and better versions of each.

He had a kit-built [Kossel](https://reprap.org/wiki/Kossel) 250 from 5+ years ago. He'd bought a bunch of upgrades for it, but the RAMPS 1.4 board went haywire a few years ago, so the heating bed stopped turning off. Since the time he built it, 3D printers have become much more inexpensive, and much easier to maintain.

{{< image src="20200808_kossel-before.jpg" caption="A dormant Kossel 250 delta 3D printer. The dust was also free." >}}
{{< image src="20200811_kossel-cleaned-powered.jpg" caption="All cleaned up. Now what??" >}}

<!--more-->

## Start small

The first thing I did after cleanup was take inventory of the parts, and make a rough plan of attack. Along the way I found and collected a glossary of terms, as well as likely documentation. I knew this printer was left in a usable state, albeit with the heated bed disconnected, since it wasn't responding to temperature control.

The previous owner had purchased a new control board, and many other upgrades. But I decided to see how far I could get as-is, or at least until I familiarized myself with the machine in its current, simpler state.

### Make a plan

Like anything sufficiently complex and interesting, it's critical to identify a smaller near-term goal. I found plenty of interesting things to chase later, but it took frequent, sometimes audible reminders to myself to stay focused. It's all too easy to fall into that false sense of productivity from premature or excessive research. 

1. Install and configure Arduino Studio to recognize and control the RAMPS 1.4 board.
1. Learn just enough about Marlin Firmware for delta 3D printers to create a new configuration for this machine.
1. Print a basic calibration or test object.
1. Decide what to do next.

### Stick to the plan

...with relatively few diversions, for me anyway.
* Installed OctoPi (for Octoprint) on the supplied Rasberry Pi 2 for a friendlier interface and remote monitoring via webcam on my local network. It was amazingly trivial to set up.
  * **NOTE:** on my laptop, the recommended "Etcher" utility for flashing the SD card repeatedly caused BSOD. I had better luck with the Raspberry Pi Imager.
* I needed to dig quite a bit to learn and configure the basic details and dimensions that are important for a delta 3D printer.
  * This was helped by some Marlin commands for reading the current configuration. This wasn't an exhaustive list, but saved me some steps and measurements.
  * There's a fair amount of info on delta printers accross Youtube, RepRap forums, and elsewhere. However most examples use much older versions of Marlin with poorly-documented deprecations, or they're for delta kits *just* similar enough to my printer to be misleading.

## The results

***Print, change, repeat***
{{< image src="20200813_kossel-print-lineup0.jpg" caption="First print attempt and subsequent experiments." >}}

I managed to get a few prints of this [calibration cube](https://www.thingiverse.com/thing:1278865) to complete. But the first layer was shifted quite a bit, and the bed adhesion suffered. On the 4th cube from the left, I felt I'd achieved "close enough" manual bed calibration and [GCODE](https://en.wikipedia.org/wiki/G-code) temperature calibration to give the classic benchmark/torture test [3DBenchy](https://www.thingiverse.com/thing:763622) a try.

{{< image src="20200813_kossel-print-lineup1.jpg" caption="Heh, nope..." >}}

After this attempt I learned about the order of operation at play, and how settings in your model's GCODE override your printer's default configuration. I followed the recommended layer height etc for the calibration cube and 3DBenchy, to much better results.

{{< image src="20200813_kossel-print-lineup2.jpg" caption="First successful (completed) 3DBenchy... and friends." >}}

The Benchy had a few issues, but I felt confident enough that now I could try a few functional prints. The sides of the [knurled knob](https://www.thingiverse.com/thing:327062) for my printer's [RepRapDiscount Smart Controller](https://reprap.org/wiki/RepRapDiscount_Smart_Controller) turned out pretty well. The top layer wasn't great, but it was serviceable.

Then I printed the button from [this enclosure](https://www.thingiverse.com/thing:1237708) - seen above.

I switched to a filament with a more photo-friendly color, so as to see flaws in the print, and thus better solicit help from the internet. Again, the 3DBenchy turned out passable...

{{< image src="20200813_kossel-benchy3.jpg" caption="3dBenchy again, decent first layer." >}}
{{< image src="20200812_kossel-benchy-infill.jpg" caption="3dBenchy infill. I'm sure it will get old someday... not today!" >}}
{{< image src="20200813_kossel-benchy0.jpg" width="195" >}}
{{< image src="20200813_kossel-benchy1.jpg" width="195" >}}{{< image src="20200813_kossel-benchy2.jpg" width="195" >}}{{< image src="20200813_kossel-benchy-4.jpg" width="195" >}}

Emboldened by this print, I figured I'd carry on. But then the aforementioned controller case turned out like this...

{{< image src="20200812_kossel-failed-panel.jpg" caption="Yeah, that doesn't look right." >}}

I lost track of all the things I tried. Suffice to say my printer wasn't respecting the boundaries and settings I established. Either that or I misunderstood what it could handle.

{{< image src="20200813_kossel-print-lineup3.jpg" caption="Again, serviceable, but clearly poorly-tuned prints." >}}

At this point, it was clear I needed help to get better results. Either that or I just needed to step away from it for an evening. Or both.

But at least the machine was generally operational, albeit in need of more work. Not too shabby, all things considered.

## The lessons

### Specific to delta printers

1. I can see why many sources and guides I find recommend you get a delta printer as your 2nd or 3rd. If I had started from scratch, this would have been much more frustrating.
1. There is a "safe zone" below the maximum travel each tower allows. This means that after all the carriages reach the top, you should either manually or programmatically lower on the Z axis far enough such that horizontal movement won't cause a carriage's required upward movement to ram into the top endstops. This will result in the belt slipping or completely coming loose. Ask me how I know!
   * **NOTE**: A lot of guides I found assumed this didn't mention this, even though their steps assumed you were starting from scratch.

### General 3D printing

1. Read the directions.
    1. A lot of 3D models have suggested print settings. Try these first before attempting modifications.
	  1. For models that don't have a lot of directions or detail, expect to do some experimentation to find successful settings.
1. Temperature is key.
    1. Temperature affects how well your first layer adheres to the bed, and subsequent layers. As such is the foundation for the rest of the print.
	  1. A working heated bed would mean better adhesion, and probably less fiddling with other variables outside recommended settings to get similar results. A heated bed is common, and sometimes it's assumed you have one.
	  1. Your environment affects temperature. This seems obvious, but notable since printing in your garage or your chilly basement can yield wildly different results.
         * Again, this is where a heated bed would help me, since my are of the basement is pretty chilly. At least it's not next to a vent.
         * Even the reputable Prusa [recommends using an enclosure for your printer if you can](https://blog.prusaprinters.org/cheap-simple-3d-printer-enclosure_7785/). I will save that for another day.
1. Precision is critical.
    1. I was fortunate that I already had a set of calipers and precision metric rulers in a variety of sizes. I used them to measure calibration, key component sizes, and accuracy of prints.
    1. Did I already mention the importance of the first layer? If it lays down unevenly, you'll probably get curled edges or the thing will just pop right off the bed in the middle of your print.

### Personal takeaways

Not that anything you've read prior was void of opinion. But the following are specifically personal statements.

#### 3D printing in general

Even with countless situations where I was ready to scrap the whole thing, I enjoy the activity. I even enjoy the assembly, repair, and debugging. I enjoy work that has a physical result, and the unique implications therein.

While I enjoy tinkering with the machine as a hobby, I'd prefer to also have a 3D printer with stronger community support, and a more common configuration. Perhaps something more "out of the box" like a [Prusa](https://www.prusa3d.com/original-prusa-i3-mk3/) or "good by default, great with mods" option like an [Ender 3 Pro](https://all3dp.com/1/20-must-creality-ender-3-upgrades-mods/).

I appreciate what I learned in trying to tune this thing with no heated bed, and manual calibration and bed leveling. But I'm so ready to buy whatever sensors or tools are required to perform these tasks automatically. Future printers will either need to already have these, or support them as a *trivial* upgrade.

If you end up using Marlin, as I did with the RAMPS board, using [PlatformIO](https://marlinfw.org/docs/basics/install_platformio_vscode.html) or the [Auto Build Marlin](https://marlinfw.org/docs/basics/auto_build_marlin.html) VSCode plugin is a much better experience than Arduino Studio. It even compiles and uploads *way* more quickly.

#### Delta 3D printers

Would I build a delta from scratch? Probably not any time soon. Even a kit would need to be pretty solid, and with sufficient positive response from the community.

That said, deltas are said to be great for fast and high-quality prints, *once you have them tuned well*. Again, compared to cartesian printers, that may hinge more on your resilience and ingenuity to achieve.

However, delta printers are mesmerizing in their construction and in operation. It's possible that I may always want one, even if just for that reason.

#### 3D printers as a tool

Many use 3D printers purely for their novelty. I certainly don't blame them. As someone that appreciates constructive hobbies, this would certainly do the trick.

What has me really excited is the potential for small batch, custom fabrication. Even if a part needs to be machined, having the option to prototype the shape and movement via [FDM](https://en.wikipedia.org/wiki/Fused_filament_fabrication) makes a lot of sense.

I'm looking forward to using 3D printing to supplement other hobbies (RC planes), new hobbies (robotics and small electronics), and general household upkeep.

## Here's what helped

Without just dumping my build/revival log verbatim from Google Docs, here's some of the tricks and references I used the most:

### YouTube calibration guide for Kossel XL

{{< youtube id="vEmBDYcx30Y" >}}
* This printer is pretty similar to mine, as best I can tell. He still references the deprecated `DELTA_SMOOTH_ROD_OFFSET`, as most guides I've found do. I couldn't find an equivalent in recent versions of Marlin.
* The video has great explanations, and a very helpful outline with timestamps in the description.
* Watching at 1.5x speed wasn't *too* quick. It got me through the content quickly, with enough time to pause and adapt his instructions to my slightly different tools or situation.
* Something he covered that many skip is getting the extruder calibration right. [M92](https://marlinfw.org/docs/gcode/M092.html), some blue tape, and a set of digital calipers made this much easier.

### Kossel XL and Mini build manual playlist

* For some reason, perhaps since the kits are long discontinued, [this playlist](https://www.youtube.com/playlist?list=PLvkxDPeJpn0WRw8BBw0L_j8BxFcuTKI8N) is unlisted.
* I needed to refer to this a few times, mostly to make repairs, or decide if an upgrade was worth disassembly.

### Marlin Firmware documentation and tools

However good or thorough docs can be, it's rarely enough to [just link to them](https://marlinfw.org/meta/gcode/).
* Searching the docs page for `delta` let me skim what options may be unique to my printer type, or what options are cartesian-only.
* [M503](https://marlinfw.org/docs/gcode/M503.html) to report the settings already on the printer. As stated before, this gave me a slight head start based on the previous owner's configuration, or otherwise let me double-check my understanding.
* Again, I wish I'd known about [Auto Build Marlin](https://marlinfw.org/docs/basics/auto_build_marlin.html) sooner, rather than waiting forever for Arduino Studio to upload 1-line changes to `Configuration.h`.
* [Forking Marlin on GitHub](https://github.com/hitjim/Marlin) to help maintain my `Configuration.h` and use Git to easily compare my changes to the defaults. Maybe there's a better way, but this works for now.

### Other various 3D printing tools

* [OctoPi/OctoPrint](https://octoprint.org/download/): most helpful after printer is operational. It's the easiest way to load and monitor GCODE to print from anywhere on your network, or from the internet if you so choose.
* [Slic3r](https://slic3r.org/download/): an Open Source slicing tool. Turns model (STL) files into GCODE required by the printer.
* [Repetier Host](https://www.repetier.com/): some people use it as a full suite, but I mostly used it to direct-connect to the printer and test/debug calibration settings.
* UGU Stic clear glue sticks. I put a thin layer on the glass bed before prints. If the print doesn't remove too much, sometimes it's good for 2 or 3 prints. Cleans off nicely with water.
* 2x microfiber rags. One for cleaning and one for drying.
* "Sewing machine" tweezers. Great for precision picking or grabbing oozing filament from the hot end before it ruins the first layer.
* Putty knife, paint scraper, phone repair kit. All these helped in various ways to pry stubborn prints off the glass print bed.
* Precision measuring tools, like digital calipers and *metric* metal rulers of varying sizes.
* Ball-end metric hex key set. Sometimes those hex bolts are at weird angles.
* Zip ties, a variety of M3-M6 screws and nuts, and "drop-in" nuts that fit in the slots/grooves of the rails without removal.
* A build log. Use a notebook or Google Docs or something. Chances are you won't remember all the things you tried yesterday that didn't work. Save yourself some time and write a few notes at the end of a debugging session.
* USB extension cable and a folding table.

### Debugging 3D prints

These do a decent job of calling out the mosts common starting points to debugging your prints. If they don't fix your problem, it helps to have tried them when you go asking the internet for help, since they'll probably get suggested!

1. [Print Quality Troubleshooting Guide](https://www.simplify3d.com/support/print-quality-troubleshooting/) from Simplify3D.
1. [Troubleshooting Guide to Common 3D Printing Problems](https://all3dp.com/1/common-3d-printing-problems-troubleshooting-3d-printer-issues/) from All3DP.
1. [3D Printer Troubleshooting Guide](https://www.matterhackers.com/articles/3d-printer-troubleshooting-guide) from MatterHackers.

There are many others out there. Eventually the content starts to repeat itself, but is usually worth a skim for new hints. I've even found helpful videos on YouTube from searching `troubleshooting 3D prints` or `debugging 3D prints`.

After you've tried a few things, [r/FixMyPrint](https://www.reddit.com/r/FixMyPrint/) on Reddit seems to be the worth a shot. I haven't used it yet, but the notion of doing so drove me to attempt further research on my own.

### Remembering how computers work

I can't tell you how many times I forgot that Octoprint was connected to the printer's control board while trying to connect from Repetier or upload some firmware, or vice versa. You can't have more than one connection on a serial port at once. It will tell you the upload failed and you'll freak out because you didn't change anything that crazy. I re-learned this at least twice a night.

## What's next

1. Attempt install of [FSR](https://reprap.org/wiki/FSR) bed leveling solution, using [this guide](https://ultibots.dozuki.com/Guide/UltiBots+D300VS+Build+Guide/1#s70). If that is too much of a pain, maybe I'll get a [BLTouch](https://www.antclabs.com/bltouch).
1. Attempt upgrade from [RAMPS 1.4](https://www.reprap.org/wiki/RAMPS_1.4) 8-bit controller (with broken heated bed control) to the 32-bit [Azteeg x5 mini v2](https://www.panucatt.com/azteeg_X5_mini_reprap_3d_printer_controller_p/ax5mini.htm), as provided by the previous owner.
    * These boards support [Smoothieware](http://smoothieware.org/), which is an alternative solution to Marlin, and is supposedly better suited to delta 3D printers.
1. Get a different printer, full stop.
    * Again, this has been a fun ride. But I'd at least like *one* consistent, modern, and more broadly-supported solution on the workbench.

All in all, wrestling with this thing has been a blast. If nothing else it gave me a great excuse to clean up my workspace and build another workbench!
