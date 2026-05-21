+++
date = "2026-05-17T13:37:58+10:00"
draft = true
title = "Laptop Power on Charge Mod"
description = "Modding my laptop to boot when the charger is plugged in"
author = "nextredo"
categories = ["hardware"]
tags = ["hacking-modding"]
series = []
featured_image = ""
toc = true
weight = 1
+++

# Laptop Power On Charger Attach Mod
## Background
In February 2026, I received an old (from 2016) Acer laptop (model Aspire 15 F5-573G) from a friend.
As someone who has been getting into homelabbing, this seemed like the perfect little server machine.
- It has a whopping **3 drive slots** - good for RAID 1
  - 1x M.2 SATA drive slot
  - 1x 3.5" SATA drive slot
  - 1x hidden 3.5" SATA drive slot (if you swap the DVD drive for an ["Optical Bay HDD Caddy"][hdd-caddy-aliexpress])
- It has a [good enough processor imo][intel-i5-7500u] (2 core, 2.70 GHz base)
- It has actually a lot of RAM, [especially in today's economy][wikipedia-ram-shortage] (16 GiB)
- It sorta has its own UPS if you think about it (battery)
- Being a laptop, it's pretty power efficient

So... what's the catch?

## The catch
Digging around the BIOS, I noticed that there were very few settings present.
<!-- TODO BIOS settings pic or gif -->
Most notably there was no "Power on AC Attach" setting to automatically boot when the charger is connected.

This poses an issue.

What if the power goes out, the laptop drains its battery, and then the power comes back on?
I assume the answer would be that it just wouldn't turn back on.
Aggravatingly, this setting is common on a lot of other laptops - like [my other homelab laptop][lenovo-yoga-260].

### Trying to overcome this
I investigated a few ways to overcome this - each of which (and why they failed) is below:

##### Maybe a newer BIOS has that functionality?
Maybe. I tried looking for one on Acer's website - but the [driver search page][acer-driver-search] wouldn't accept my laptop's serial number, or any of the ones I found online.
It also **stupidly** doesn't just have info & downloads without needing an S/N.
Thankfully I actually made it to [the driver page][acer-driver-downloads] via one of the S/Ns I found (can't find it now though ofc).

Oddly enough however, I did find some [information online][acer-bios-1.27] that indicates later BIOSes do exist for this machine.
But since I can't find or install any of them, this idea is "myth busted".
<!-- TODO post my BIOS version -->

<!-- TODO try this advanced BIOS settings hack thing
https://community.acer.com/en/discussion/551722/wake-on-lan-aspire-v5-573g

Artificial sloptelligence summary:
Step-by-Step GuidePower Off: Shut down your Acer Aspire F5-573G completely.Apply Key Combination: Press and hold the Fn + Tab keys simultaneously, and tap the Power button once.Reboot and Enter BIOS: As the laptop turns on, immediately release the Fn + Tab keys and press F2 repeatedly to enter the standard BIOS screen.Access Advanced Mode: Once inside the standard BIOS, hold the Fn + Tab keys again and press the Power Button until it turns off. Turn the laptop back on, release the buttons, and repeatedly press F2. The Advanced tab should now be available at the top of the screen.
-->

##### Use Wake On LAN?
One setting that the bios actually does have is "Wake on LAN".
I thought about using this, but that'd require having either a router or other computer to wake it from sleep.
Maybe an Arduino, maybe a Raspberry Pi, maybe it'd be automatic, maybe it'd be manual.
Either way, I decided this was a bit of a pain and wouldn't be the option I go with.

##### Just get a new computer bro?
Well yeah, I could. But where's the fun in that?

Plus, this way I'm not just creating more e-waste, and I get to write [a cool post](#laptop-power-on-charger-attach-mod) about it.

##### How about modding it?
Well, maybe that might just work.

Like how most good things start, I turned to google and ended up on reddit (/s).
[This post][reddit-post] sounded pretty promising, so I got to work.

## Modding it
### The plan
- 5v relay off USB
- 20v relay off charger

### Getting the parts
- 5v relay
- 20-ish volt relay
- wire, soldering iron, connectors, wire stripper, multimeter

### Wait, where's the power button?
- pwr button is part of keyboard
- looked for different traces on the mobo --> keyboard connector
- found a pair that changed from open circuit to 200-ish ohms when pwr btn pressed
- manually shorted them (with a resistor) to check

### Now where's the power come from?
- found where power comes into the mobo

### Wiring it up
- wired it all up
- tested it with the shell off
- may have accidentally shorted one of the relays 👀
- put holes through the case since I can't fit the relays inside
- stuck the relays on the case

### Fini
- works!

#### Problem?
- Doesn't turn back on if you manually turn the server off
- Could be a good thing
- But it essentially just holds the power button down for good, which is weird
- So need to unplug & re-plug it to power back on
- Could solve this with a power point timer for this laptop and others
- Also means you can't put this laptop right-side up ;(

---

- Got this old laptop from a friend
- Good because it has a battery, and 3 drive slots
- Super useful for RAID 0 drives + 1 boot drive, also 2x sodimm slots
- Great as a server
- However, AC power issue
- Could do
  - Wake on LAN
  - Solenoid and power relay
  - Double relay strat

- first test went well
  - minor sparks, nothing seems broken
  - cos I had the 5v relay sitting on a metal plate
  - noticed the PC doesn't turn back on if manually turned off
  - could solve this with a powerpoint timer that stops for a minute or so each day
  - or, could try add a capacitor + inrush resistor to attempt to solve it

# Resources

# TODO
- Mod done on 13/May/2026
- [ ] Pics of bios screens
  - Blur out serial numbers
  - Show that there's no pwer on AC option
- [ ] Reply to the Reddit post
  - Link this static site blog post
  - Thank bro


<!-- Links -->
[hdd-caddy-aliexpress]: https://www.aliexpress.com/item/1005002573524607.html
[intel-i5-7500u]: https://www.intel.com/content/www/us/en/products/sku/95451/intel-core-i77500u-processor-4m-cache-up-to-3-50-ghz/specifications.html
[wikipedia-ram-shortage]: https://en.wikipedia.org/wiki/2024%E2%80%93present_global_memory_supply_shortage
[lenovo-yoga-260]: https://web.archive.org/web/20160818005603/http://shop.lenovo.com/us/en/laptops/thinkpad/yoga-series/yoga-260/
[acer-bios-1.27]: https://community.acer.com/en/discussion/538838/bios-upgrade-to-latest-1-27-acer-aspire-f5-573g-no-mssd-present-and-battery-not-working
[acer-driver-search]: https://www.acer.com/au-en/support/drivers-and-manuals
[acer-driver-downloads]: https://www.acer.com/au-en/support/product-support/Aspire_F5-573G
[reddit-post]: https://www.reddit.com/r/homelab/comments/14swppq/comment/mpd7dqp/?utm_source=share&utm_medium=mweb3x&utm_name=mweb3xcss&utm_term=1
