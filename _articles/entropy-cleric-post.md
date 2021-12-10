---
id: 1
title: "Entropy Cleric"
subtitle: "Custom synthesizer and algorithmic music system in MaxMSP"
date: "2020.07.01"
tags: "generative, max"
---
The Entropy Cleric is a synthesizer I built in MaxMSP, including standard synth functionality – MIDI input, oscillators, portamento, variable filter, ADSR, LFO, overdrive, delay, presets – as well as a simple note generation system. This project was a very enjoyable way to gain a practical understanding of synth signal flow and individual module functionality, which will be useful for my 3rd year group project – building a analog Eurorack modular synth.

Building a randomised algorithmic system which routes notes to different oscillators shifting octave, interval, rhythm, and velocity was a fun way to expand upon the material in the course. My work 'Pareidolia', available on [Soundcloud](https://soundcloud.com/0x0c/pareidolia), was produced using the Entropy Cleric and is intended to explore human-algorithm collaboration with live sound design performed on the synth as it runs.

The interface for the Entropy Cleric is shown below, each part labelled. More detailed instructions for playing it are included in the Max [patch](https://github.com/haelyons/MaxMSP-Experiments).

![Entropy Cleric](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/main%20page.png)


## Spicey™ Note Generator
The algorithm itself is remarkably simple. The parameters are toggles for generation and to enable automatic cycling through chords at a set interval, integer tempo value, defaulting at 300 BPM, and selected notes on the MIDI keyboard (top left).

The processes that guide generation can be divided into 3 sections: tempo, velocity and note selection, which will be expanded upon below with illustrations from the patch. The use of multiple oscillators and routing _note on_ values is what allows polyphony in this algorithm.

![Spicey™](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/spicey%20generation.png)


### Temporal Qualities & Velocity
The `tempo` object sends out a `bang` which triggers the notes. The 'tempo' parameter – shown as a 300 above – controls the `tempo` object which generates note values to send and be routed to oscillators, at 300 BPM.

There is a separate timer `metro 500` which triggers a random selection between 1-3 (inclusive) at 500 BPM through use of the `random` object. This selection sets the 'beat multiplier', described in the Max8 documentation:

> The 3rd inlet is a beat multiplier, which can lengthen the amount of time taken for one beat. It slows the tempo down by a factor of N. For example, a multiplier of 2 will make tempo send out its output half as fast.[^1]

Velocity is also generated with `random` object, range 1-127 (incl.), triggered by the `tempo` object. It is sent through the 'Key Velocity Info' inlet, and processed along with the keys selected on the MIDI keyboard (top left).

### Note Selection
Note decision making is based around taking the currently sustained notes – those selected on the MIDI keyboard (top right) – and randomly assigning them an output (Over, OSC1, OSC2) and an octave / interval. The grey box over `zl scramble` shows the currently held notes, and randomises the order every time a note is sent. The `bang` that triggers this (from the original `tempo` object) is what triggers the output and octave selection systems. These  systems both use the `gate` object in combination with randomisers (1-3 and 1-8 ranges respectively). The `gate` object acts as a simple switch. Max8 documentation:

> Use gate~ to route an input signal at the second inlet to one of several outlets, or to no outlet at all. When there is only one outlet (the default case), it acts as a simple switch. Unlike the Max gate object, any outlet which is not selected outputs a signal composed of zero values.[^2]

As such the randomised number refers to which oscillator receives the note value – Over, OSC1, OSC2. The octave system works in a similar way, each outlet of the gate sending the note value through an integer operation which transposes up or down a certain number of octaves. 1 octave = 12 notes, up 36 notes = 2 octaves. The empty number boxes can be used to add intervals, though currently increase the chance that the note plays on its default octave.

## Reflection on Generative Music
I find the generated output of this synthesizer fascinating, both because it is essentially through composed with no overarching structure, and because the sound can be radically modified while it is generating. This live setup makes the performer feel they are playing the music and leaves them to focus on the intersection between the timbral and melodic. This calls back to Brian Eno's example of wind chimes as a form of generative music: "the only compositional control you have over the music is in the choice of notes that the chimes will sound."[^3] While this quote does not wholly apply to this work as this generative process allows for significant timbral modification which is arguably a form of compositional control, it is fascinating that such a seemingly complex result can be produced with so few control parameters.

The cover art for this work was mutated from the picture 'Holz Geist Gesicht' by Usien. It shows a tree with a cut cross-section resembling a human face.[^4] Using 'pixeldrifter' software – a series of algorithms that manipulate individual pixels based on their luminescence – this face was transformed into the work below; another dig at humans, algorithms, and their intersections, as with the word 'Pareidolia'.[^5]

![Spicey™](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/pareidolia.jpg)

[^1]: https://docs.cycling74.com/max8/refpages/tempo?q=tempo
[^2]: https://docs.cycling74.com/max7/refpages/gate~
[^3]: https://intermorphic.com/sseyo/koan/generativemusic1/
[^4]: https://commons.wikimedia.org/wiki/File:Holz_Geist_Gesicht.JPG
[^5]: https://pixeldrifter.tumblr.com/
