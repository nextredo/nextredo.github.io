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
Like the post says, I planned to have 2 relays - a 24v one and a 5v one.
The 24v relay (normally open) is powered by the charger. It shorts the power button.
The 5v relay (normally closed) is powered by the USB. It opens the 24v relay.
This way, we have the following sequence of events:
1. The power cable is plugged in
2. This closes the 24v relay, shorting the power button
3. The laptop begins to turn on
4. The USB ports become powered
5. This opens the 5v relay, disconnecting the 24v relay, which opens the power button
6. Laptop boots as normal

### Getting the parts
I already had a cheap "Arduino 5v relay module", so all I needed was the 24v one.
Thankfully, it was easy enough to find at a local electronics store.
Other things like wire, a soldering station & materials, a multimeter etc. I already had.

First thing I made sure to check was that it actually activated consistently with the 19v charger.
Seemed to work ok.

### Wait, where's the power button?
Here's the first challenge of the implementation.
Unlike most laptops with a dedicated power button, this one has the power button as the top-right key on the keyboard.
<!-- TODO keyboard pic -->

This could be a bit of a pain, because I assume it means the power button is connected to the motherboard through the keyboard connector,
so its PCB traces will be less obvious than if the power button wasn't part of the keyboard.

<!-- TODO circuit board pic -->

Upon opening the laptop up, I looked for the keyboard connector.
I assumed it'd be a cable with a lot of pins somewhere, and one at the bottom of the mobo fit that description nicely.
I watched a keyboard replacement video for some ideas on how to find the power button PCB traces,
but thankfully it also gave me certainty that I actually had found the keyboard connector.

<!-- TODO kb connector pic -->

Looking at the connector, I noticed 2 pins on the right edge of it that had different PCB traces to the others.
Since I assumed the power button wouldn't be part of the normal keyboard interface
(nor would it be routed to the same spot on the mobo), I checked for continuity between the two pins with my multimeter.

Toggling the power button on and off while doing so, I found that the pins were open-circuit when not pressed, and about 236Ω when pressed.

I double checked this was actually the power button by shorting the two by hand via a 225Ω resistor - and sure enough the laptop turned on.

### Now where's the power come from?
This part wasn't so hard, since the barrel jack for power goes straight to a nice big connector on the mobo via some nice red and black wires.

### Wiring it up
<!-- TODO 5v relay pic -->
#### 5v relay connections
| Relay pin | Relay pin use | Laptop connection |
|---|---|---|
| s | Signal - for controlling the relay coil | USB +5v |
| + | Power - for providing power to the module | USB +5v |
| - | Ground - power's other half | USB ground |
| NC | Normally closed - the pin of the relay's "switch" end that is connected to COM when the relay coil is off | Charger +19v |
| NO | Normally open - the opposite of NC (connected to COM when the coil is on) | N/A |
| COM | Common - the central part of the relay's "switch" end | The 24v relay's positive coil input |

<!-- TODO 24v relay pic -->
#### 24v relay connections
|---|---|---|

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
