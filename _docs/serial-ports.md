---
layout: defaults
title: About Serial Ports
published: true
---
#What we know about USB Serial Ports

## Why we care

Many Diabetes devices use serial ports to communicate with the computer. Serial ports have existed for a very long time -- USB came along much later. Many USB devices simply emulate a serial port to do the communications.

There is a chipset by a manufacturer called FTDI that makes it easy for hardware designers to incorporate a USB serial port into their devices.

It became popular enough that in OSX version 10.9, Apple embedded support for FTDI-based serial ports into the operating system, so that no device drivers were required.

The problem with that for a device manufacturer is that if you just use the generic driver, then your device won't be automatically identified when you plug it in. So most manufacturers make a special cable that uniquely identifies their device, and then write a custom driver for it.

Basically, many of the devices we talk to use serial communication over USB, and we have to deal with it.

## What are the problems?

### Cables

In the context of serial communications over USB, there are basically two ways to design a cable interface:

    a) [PC USB Port] <- [USB plug with FTDI chip] ====serial cable==== [plug] -> [device]

    b) [PC USB Port] <- [USB plug] ====USB cable==== [mini USB plug] -> [FTDI chip inside device]

The first (a) has a communications chip in the end of the cable that plugs into the PC. It allows the diabetes device to be simpler since no extra chip is needed in the device itself.

The second style, (b), requires more smarts and circuitry on the device, but allows it to be used with a standard USB cable.

Asante and LifeScan and others use cable style (a) -- the device end has a standard 1/8" phone plug (looks like a headphone plug) for their serial connector -- which allows us to use a standard FTDI cable we can buy over the counter to talk to it.

These manufacturers also have a custom cable that we don't know how to use yet (more on that later).

Abbott also uses strategy (a) but chose to double-up on the strip connector with a special cable that plugs into the strip slot, so we *must* use the Abbott cable. On the PC, the Abbott cable has a driver that makes it look like a serial port. On the Mac, they use custom software and don't need a driver.

Version (b) is the approach Dexcom has taken -- you can use any USB-to-micro-USB connector (like the cable for a Kindle or many cell phones)

### Port identification

Serial ports move around. To talk to a specific device, you need to know its name -- but a given device's name is not inherent -- it gets assigned by the operating system when you plug it in.

On Macs, devices get TWO names (one starts with tty and the other starts with cu). They're almost equivalent (details don't matter for us, really). Examples are: cu.usbmodem373 or cu.usbserial-f31g.

On PCs, devices get a name that looks like COM2 or COM4. Again, the number can vary.

On Macs, the same physical device seems to show up with the same name every time you use it. But multiple Dexcoms, for example, would have different port names.

On PCs, modern versions of Windows try to use the same port for the same device -- but there are lots of things that can make that not be true.

We are especially going to have problems, I believe, in clinics, where they may plug in hundreds of different devices over the course of a month.

And finally, not all serial ports on a machine are associated with a diabetes device. I have four ports on my Mac besides any that I plug in related to this project.

### Device identification

There's no standard way to tell what kind of device is on the other end of a serial connection. Every device has its own individual protocol -- the language it speaks.

This brings up an obvious idea - why can't we try talking to each device we know about and see what it responds to? The problem is that we don't have any guarantees about how a device will respond to something it doesn't understand. Perhaps a story will help:

> It is of course well known that careless talk costs lives, but the full scale of the problem is not always appreciated.
> For instance, a human (see Earth) named Arthur Dent who, because of a Vogon Constructor Fleet, was one of the last two humans in the Universe at the time, once said "I seem to be having trmendous difficulty with my lifestyle." At the very moment that Arthur said this, a freak wormhole opened up in the fabric of the space-time continuum and carried his words far far back in time across almost infinite reaches of space to a distant Galaxy where strange and warlike beings were poised on the brink of frightful interstellar battle.
> The two opposing leaders were meeting for the last time.
> A dreadful silence fell across the conference table as the commander of the Vl'Hurgs, resplendent in his black jewelled battle shorts, gazed levelly at the the G'Gugvuntt leader squatting opposite him in a cloud of green sweet-smelling steam, and, with a million sleek and horribly beweaponed star cruisers poised to unleash electric death at his single word of command, challenged the vile creature to take back what it had said about his mother.
> The creature stirred in his sickly broiling vapour, and at that very moment the words "I seem to be having tremendous difficulty with my lifestyle" drifted across the conference table.
> Unfortunately, in the Vl'Hurg tongue this was the most dreadful insult imaginable, and there was nothing for it but to wage terrible war for centuries.
    -- *Douglas Adams*, _The Hitchhiker's Guide to the Galaxy_

Our problem is that we don't know which devices might be Vl'Hurgs that will take offense at an innocent query. It's risky to start probing medical devices with commands they might misinterpret.

For the first couple of devices, it's not such a problem -- we can reasonably find a way to distinguish them safely. But as we add more devices, it will become more and more difficult to safely tell them apart. The complexity goes up with the square of the number of devices we're trying to detect.

## So what can we do?

(This section is not finished)

* We can try to autodetect as many devices as we can
* We can get the user's help with identifying devices
  * When a new device gets plugged in, we can ask for help identifying it (rather than trying to guess)
  * We can ask users to help us with identification by plugging in a device while we are watching, so we can note which port it's been assigned
  * The Asante SNAP pump actually sends a beacon every 5 seconds, which can let us identify it if we're willing to wait.
* We can differentiate between end users who have a small number of devices they use a lot, and clinics who have a large number of devices that are plugged in at different times. The clinic users may need to help us more -- but they can be trained.
* We will have to provide a port assignment interface somewhere (it can be hidden in settings) that can be used to map devices to ports. (Idea -- make it look like a patch panel?)
* We can ask users to identify the possible devices and maybe even the order in which we try them, to minimize conflicts.
