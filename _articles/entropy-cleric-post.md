---
id: 1
title: "Music Generation I"
subtitle: "Building an algorithmic note system in MaxMSP"
date: "2020.07.01"
tags: "generative, max"
---
The Entropy Cleric is a synthesizer built in MaxMSP demonstrating knowledge of typical synthesizer signal flow and individual module functionality – MIDI input, oscillators, portamento, variable filter, ADSR, LFO, overdrive, delay, and presets.

It originally featured an 'entropy' counter – a simple countdown from 12,960,000 (3600^2) mapped & scaled to a number of parameters to progressively degrade sound quality into noise.  This was replaced with the 'Spicey™ Note Generator' after my initial research into generative music, which showed me that a simple algorithm could produce a complex and beautiful result. 'Pareidolia', generated in the Entropy Cleric and available on [Soundcloud](https://soundcloud.com/0x0c/pareidolia) is intended to explore human-algorithm collaboration, with live sound design performed on the synth as it generates a melody and harmony.

![Entropy Cleric](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/main%20page.png)

## Spicey™ Note Generator
The algorithm itself is remarkably simple, requiring: a toggle to enable generation, a tempo value (default 300 BPM) and selected notes on the MIDI keyboard (top right). The processes that guide the generation can be divided into 4 sections: Overtone note, OSC1 note, OSC2 note, and Velocity. The use of multiple oscillators is what allows polyphony within this algorithm.

![Spicey™](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/spicey%20generation.png)

### Temporal Qualities & Velocity
While tempo is a control parameter for this generation, there is a separate timer `metro 500` which triggers a random selection between 1-3 (inclusive) at 500 BPM. This selection sets the 'beat multiplier', described in the Max8 documentation:

> The 3rd inlet is a beat multiplier, which can lengthen the amount of time taken for one beat. It slows the tempo down by a factor of N. For example, a multiplier of 2 will make tempo send out its output half as fast.[^1]

This creates regular, small variations in tempo which I found quite satisfying, though variation of note length (4th inlet) could have provided even more variation in the generation's temporal qualities.

Beyond this the above picture is quite clear – `tempo` object sends out a `bang` which trigger the notes. Velocity is simply generated using a 0-128 randomiser triggered every time a note value is sent out.

### Note Selection
Note decision making is based around taking the currently sustained notes – those selected on the top right MIDI keyboard – and randomly assigning them an output (Over, OSC1, OSC2) and an octave / interval. The grey box over `zl scramble` shows the currently held notes, and randomises the order every time a note is sent. This note bang is also what triggers the output and octave selection systems. The options for octave selection can be seen on the bottom right (+12, +24, +36, -12).


## Pareidolia
I find the generated output of this synthesizer fascinating, both because it is essentially through composed, with no overarching structure, and because the sound can be radically modified while it is generating. This live setup makes the performer feel they are creating the music, and leaves them to focus on the intersection between the timbral and melodic.

The cover art for this work was mutated from the picture 'Holz Geist Gesicht' by Usien. It shows a tree with a cut cross-section resembling a human face.[^2] Using 'pixeldrifter' software – a series of algorithms that manipulate individual pixels based on their luminescence – this face was transformed into the work below, a final dig at nature, humans, and algorithmic interaction.[^3]

![Spicey™](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/pareidolia.jpg)


[^1]: https://docs.cycling74.com/max8/refpages/tempo?q=tempo
[^2]: https://commons.wikimedia.org/wiki/File:Holz_Geist_Gesicht.JPG
[^3]: https://pixeldrifter.tumblr.com/
