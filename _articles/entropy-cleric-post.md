---
id: 1
title: "Entropy Cleric"
subtitle: "Custom synthesizer and algorithmic note system in MaxMSP"
date: "2020.07.01"
tags: "generative, max"
---
The Entropy Cleric is a synthesizer I built in MaxMSP, including standard synth functionality – MIDI input, oscillators, portamento, variable filter, ADSR, LFO, overdrive, delay, presets – as well as a simple note generation system. This project was a very enjoyable way to gain a practical understanding of synth signal flow and individual module functionality, which will be useful for my 3rd year group project – building a analog Eurorack modular synth.

Building a randomised algorithmic system which routes notes to different oscillators shifting octave, interval, rhythm, and velocity was fun way to expand upon the material in the course. My work 'Pareidolia', available on [Soundcloud](https://soundcloud.com/0x0c/pareidolia), was produced using the Entropy Cleric and is intended to explore human-algorithm collaboration with live sound design performed on the synth as it runs.

The interface for the Entropy Cleric is shown below, each part labelled. More detailed instructions for playing it are included in the Max [patch](https://github.com/haelyons/MaxMSP-Experiments).

![Entropy Cleric](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/main%20page.png)


## Spicey™ Note Generator
The algorithm itself is remarkably simple, requiring:
– A toggle to enable generation
– A toggle to enable the pre-selected chords that change every minute
– A tempo value (default 300 BPM) and selected notes on the MIDI keyboard (top right).

The processes that guide the generation process can be divided into 4 sections: tempo, velocity, note selection and selected keys. The use of multiple oscillators is what allows polyphony within this algorithm.

![Spicey™](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/spicey%20generation.png)


### Temporal Qualities & Velocity
While tempo is a control parameter for this generation, there is a separate timer `metro 500` which triggers a random selection between 1-3 (inclusive) at 500 BPM. This selection sets the 'beat multiplier', described in the Max8 documentation:

> The 3rd inlet is a beat multiplier, which can lengthen the amount of time taken for one beat. It slows the tempo down by a factor of N. For example, a multiplier of 2 will make tempo send out its output half as fast.[^1]

This creates regular, small variations in tempo which I found quite satisfying, though variation of note length (4th inlet) could have provided even more variation in the generation's temporal qualities.

As shown in the picture above, the `tempo` object sends out a `bang` which triggers the notes. Velocity is simply generated using a 0-128 randomiser triggered every time a note value is sent out.


### Note Selection
Note decision making is based around taking the currently sustained notes – those selected on the MIDI keyboard (top right) – and randomly assigning them an output (Over, OSC1, OSC2) and an octave / interval. The grey box over `zl scramble` shows the currently held notes, and randomises the order every time a note is sent. This note `bang` is also what triggers the output and octave selection systems. The options for octave selection can be seen on the bottom right (+12, +24, +36, -12).


## Reflection on Generative Music
I find the generated output of this synthesizer fascinating, both because it is essentially through composed with no overarching structure, and because the sound can be radically modified while it is generating. This live setup makes the performer feel they are playing the music and leaves them to focus on the intersection between the timbral and melodic. This calls back to Brian Eno's example of wind chimes as a form of generative music: "the only compositional control you have over the music is in the choice of notes that the chimes will sound."[^2] While this quote does not wholly apply to this work as this generative process allows for significant timbral modification which is arguably a form of compositional control, it is fascinating that such a seemingly complex result can be produced with so few control parameters.

The cover art for this work was mutated from the picture 'Holz Geist Gesicht' by Usien. It shows a tree with a cut cross-section resembling a human face.[^3] Using 'pixeldrifter' software – a series of algorithms that manipulate individual pixels based on their luminescence – this face was transformed into the work below; another dig at humans, algorithms, and their intersections.[^4]

![Spicey™](https://raw.githubusercontent.com/haelyons/Website-Content/master/entropy%20cleric/pareidolia.jpg)

[^1]: https://docs.cycling74.com/max8/refpages/tempo?q=tempo
[^2]: https://intermorphic.com/sseyo/koan/generativemusic1/
[^3]: https://commons.wikimedia.org/wiki/File:Holz_Geist_Gesicht.JPG
[^4]: https://pixeldrifter.tumblr.com/
