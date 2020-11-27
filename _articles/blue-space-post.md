---
id: 0
title: "Blue Space"
subtitle: "Building a real-time granulator in MaxMSP"
date: "2020.6.08"
tags: "granular, max"
---
## Aims & Overview
In performing harp, I have often felt limited by the timbral possibilities presented in modern DAWs
without the need for extensive post processing which cannot be performed in a live setting. Given time any number of transformations are possible; time stretching, texture generation, sample rate automation, etc. In my previous harp performances, the audio output was mostly playback from pre-recorded and arranged material with the live harp as a lead instrument, but one overshadowed by the extensive post processing used – and necessary – to create the rest of the material. As such, Blue Space provides easy access to a number of rich effects which can evolve over time or be controlled entirely by the user through MIDI and a streamlined set of controls, all in real time.

What is a granular synthesis, or a granulator? As defined by Curtis Rhodes:

> "Granular synthesis deals with sound at a 'quantum' level: the sonic atom being the individual sample (any one of the 44100 taken in a second at the standard sampling rate)."[^1]

This is applied here by taking position information from a selection within a buffer, which holds 10 seconds of live or recorded sound. This position is multiplied by a duration in milliseconds to create a 'grain' of sound. 8 grains are played simultaneously with modified phase information and noise added to the original sample position to create a more varied result. This means from any point, we can generate a never-ending sound which reflects the characteristics of that point.

![overview](https://raw.githubusercontent.com/haelyons/Website-Content/master/BLUE%20SPACE.png)

## Granular Engine
The heart of this operation is the grain engine. The one used here is from Nobuyasu Sakonda's 'Granular Synthesis 2.5' patch released in 2000. As such Blue Space serves as a dedicated expansion to allow live input into GS 2.5, along with additional features to complement live performance (random reposition, MIDI pos control). The 2 primary obstacles in achieving real-time functionality were creating a live buffer that displays 10 seconds of audio with simple controls, and removing artefacts created by the grain engine's envelope system (replaced).[^2]

![engine](https://raw.githubusercontent.com/haelyons/Website-Content/master/blue%20space/Blue-Space--Grain-Engine.jpg)

### Live Buffer
The live buffer has 4 simple options.
* Play: loops through existing audio in the buffer.
* Manual: freezes the currently selected point(s). This freeze can be repositioned using the cursor, random repositioning, or position randomisation (all options on the main interface).
* Record: records audio from the chosen input source through the buffer until turned off. Recording can be done with the freeze of another point still playing (Manual), or the previous content of the buffer still playing (Play).
* Live Record: an interesting late addition, live record sets the 'freeze' point as the recordhead while recording, and creates the truest form of 'live granulation'.

While looping a `buffer~` is simple in Max, recording based on a trigger object while overwriting the previous content of the buffer, and not creating noise in the process, was rather more difficult. The `karma~` external by Rodrigo Constanza was useful in clarifying this process, though my implementation is in native Max.[^3]


### Window (Envelope) Generator
This patch uses the `sah~` object (which is used for sample hold) to pass parameter changes in sync with the `phasor~` ramp, avoiding artefacts and glitches completely. To include this engine into my patch however, I had to rework the envelope system; while the use of `sah~` objects is effective in removing artefacts, it also limits the envelope possibilities. Because hamming or gauss windows don't have zero crossings, they aren't recognised by `sah~`. Thus, I opted to use a file based system and manually edited the envelopes in Audacity to have zero crossings.[^4]

### Conclusions
As such, Blue Space allows for significant timbral sound modification, but lacks several features which would allow it to become a standalone performance engine, rather than a live sound processing patch. For example, the inclusion of overdubbing capability and an adjustable looping period would give the performer much more control in their output. Porting this into a Max for Live device, a more immediate possibility, would let the performer run multiple granulators concurrently, each with different settings as part of a larger project. Lastly, other granulation programs have a greater degree of customisability in the size & quality of grains, and the way in which these grains can be played back, features which could be extremely useful in a live context.

![window](https://raw.githubusercontent.com/haelyons/Website-Content/master/blue%20space/Blue-Space--Grain-Window.jpg)

[^1]: Roads, C., 2004. Microsound. 2nd ed. Cambridge, Mass.: MIT Press, pp. 85-118.
[^2]: Sakonda, Nobuyasu, "Maxmsp Patch Downloads", Formantbros.Jp, 2000 <http://formantbros.jp/sako/download.html>
[^3]: Constanzo, Rodrigo, Karma~, version 1 (http://www.rodrigoconstanzo.com/karma/, 2006)
[^4]: Alessandretti, Stefano, "Abstraction Of A Real-Time Granulation System Built Into The Max/MSP Environment", Independent, 2020, pp. 1-4
