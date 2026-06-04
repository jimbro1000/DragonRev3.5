# DRAGON 32 CPU Rev 3(.5) #

This project contains the design for a hugely upgraded version
of the Dragon 32 (technically a Dragon 64 for pedants)

![Render of reproduction board](Dragon32Rev3.5.png)

The basis of the design is the Dragon 32 recreation that I have
been refining for some time. The peripheral IO remains as originally
specified by Dragon Data but the ram, address multiplexer and video
display generator or implemented on a separate plug-in component.

Technically the original ICs could be implemented on the plug-in and
you would be left with an original piece of hardware (in terms of
function). I already have a board for that though...

The benefit of the plugin board is to implement the complex (and
expensive) fabrication on a smaller device and abstract away the
upgrades that are contained.

Almost every passive component and supporting logic ICs have been
switched to surface mount technology. This means the board can be
part assembled at relatively low cost, leaving just the major ICs,
ports, connectors and relay to be hand assembled.

## Core Design ##
### Main Board ###

The main board replicates the footprint of the original D64 PCB.
Ports and fixings are all in their original positions.

The CPU lives on the main board with any speed variant supported
including the HD63C09. The onboard ROM is integrated into a single 
chip holding both banks of the D64 ROM image.

All IO (except video) is performed by the original hardware so the 
two PIAs and UART are retained.

The device select signals for peripherals has been disambiguated
from two devices which repeat the same four addresses eight times, 
to sixteen devices, each with four mapped addresses.

* $FF00-$FF03 (P0a) is still the original PIA0
* $FF04-$FF07 (P0b) is the original UART
* $FF20-$FF23 (P1a) is still the original PIA1

These three blocks maintain compatibility with the original ROM
software.

The other blocks (P0c-P0g and P1b-P1g) are unused and can be
adopted for other hardware (see audio)

### Core Board ###

The core board provides RAM, Video and SAM functionality. The
minimum version of this would be the original RAM, VDG and SAM
chips which effectively reproduces the original D64 specification.

At this time two upgrade core boards exist. The first is a mild
upgrade that provides 2MB of RAM, a SAMx implementation and an
upgraded VDG that replicates the video functionality of the 
CoCo3 GIME chip

The second core board provides all of the capabilities of the
first board but adds a soft 6809 CPU that can operate alongside
or instead of the CPU on the main board. This second CPU can
operate at much higher speeds than the original, match the
speed of the original or be completely disabled. When both
CPUs are operating the second CPU can be completely detached
from everything except RAM and running on an independent
clock cycle, or it can share the full bus with the hardware
CPU utilising the bus mastering signals.

The second core also introduces a DMA capability that enables
very fast memory copy actions, running independently of either
CPU.

The video capabilities of the second core add sprite handling
and tile based video modes, all in a layered video system that
allows tiles, bitmaps and sprites to exist on the same display.

For details of the core implementations see the separate
hardware and firmware repositories.

### Audio Board ###

The main board will (but does not currently) provide a socket
for an audio upgrade. The proposal is to utilise four or six
peripheral address blocks to provide an interface to the audio
board. Each block effectively represents a single audio device.

The multi device approach should allow the audio board to 
generate a rich stereo output with multiple audio waveforms
per stereo channel.

Optionally I want to look at live mixing of the audio to allow
user selection of which waveform ends up on which channel. This
would likely consume another peripheral address block.

Following this approach would permit the use of original sound
generator chips but has the problem of identifying the source
of interrupts.

An alternative is to define a soft audio generator in a MCU
or CPLD that focuses all of the IO into a single hardware
device (possibly still using multiple peripheral IO blocks)
but potentially makes the hardware interface non-standard.
If a suitable open source firmware definition can be
identified, this is likely to be the better option.

An example of this would be the [YM2149 PSG System](https://github.com/nockieboy/YM2149_PSG_system)
that implements a simulation of dual YM2149s (or 
AY-3-8910s) with stereo mixer and bass and treble controls.

Due to the limited external IO required this could be implemeted
on a relatively cheap, modest FPGA device rather than the main core
board which requires a significant level of IO connectivity.

## Modifications ##
### Joysticks ###

The two joystick ports provide a second fire button capability
wired to consecutive keyboard rows, as per the Tandy CoCo. 
Unless a two button joystick is used this make no difference to
the operation of the computer. It also requires the use of 6-pin 
DIN sockets, these are pin compatible with the 5-pin originals
so a regular single button joystick can still be used.

The two extra buttons can also be disabled by omitting L3 and L4

## License

This project and all its hardware design are licensed under
creative commons [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.en)

__You are free to:__

Share - copy and redistribute the material in any medium or format

Adapt - remix, transform, and build upon the material

The licensor cannot revoke these freedoms as long as you follow the 
license terms.

__Under the following terms:__

Attribution - You must give appropriate credit, provide a link to the 
license, and indicate if changes were made. You may do so in any 
reasonable manner, but not in any way that suggests the licensor 
endorses you or your use.

NonCommercial - You may not use the material for commercial purposes.
