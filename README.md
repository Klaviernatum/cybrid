# Cybrid
**Cybrid** is an open-source MIDI controller.

## Introduction
**Cybrid** is based on a DIY project Evgeni Kumanov
(AKA ***Cyber**Gene*) created in 2020 for turning a grand piano action
into a MIDI controller. In the digital piano terminiology, instruments with a
real wooden piano action are called ***hybrid*** pianos, hence the name
Cybrid (from CyberGene and hybrid), as one member on the PianoWorld forums suggested. Here's a demo
of the finished controller:

<a href="http://www.youtube.com/watch?feature=player_embedded&v=x7ZbjIRRwVg
" target="_blank"><img src="http://img.youtube.com/vi/x7ZbjIRRwVg/0.jpg"
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

In its initial state this GitHub project contains just the PCB design,
the software and the mechanical considerations/descriptions of that DIY
project. It may be used as it is for anyone willing to recreate that
project or can evolve into a more advanced and improved design that can
be used for other (hybrid or regular) MIDI-controllers, etc.

## Brief description
The goal of the project is to detect the velocity of the hammers and
turn it into MIDI velocity. This is realized by using
[Vishay CNY70](https://datasheet.octopart.com/CNY70-Vishay-datasheet-5434663.pdf)
optical sensors and
[Texas Instruments LM339](https://www.ti.com/lit/ds/symlink/lm2901.pdf)
voltage comparators.

The main principle has been suggested by Marcos Daniel, a member of the
PianoWorld forums.

* The CNY70 is an optical proximity sensor that consists of a photoemitter (a LED) and a phototransistor. The phototransistor will change its collector current depending on the proximity of subjects to the sensor
* By measuring the voltage drop created by the collector current on a resistor and comparing it to a reference voltage, we will know whether the subject is within a predefined proximity.
* A voltage comparator is a device that has two voltage inputs and a digital output. One of the inputs is a reference voltage selected through a trimpot and the other input is the phototransistor output voltage. If the phototransistor output voltage is higher than the reference voltage, the output of the comprator will be digital 1, otherwise it will be digital 0.
* Thus we can turn the continuous analog sensor signal into discrete digital binary value that is determined by whether the subject is closer than a predefined distance

This is important because the LM339 voltage comparator is very fast when comparing two voltages and switching its digital state takes a few nanoseconds and we will have a very fast detection of distance, so we won't need to use slow analog-to-digital conversion. (As you will see in the following paragraphs, scanning through all the 88 keys of a piano, i.e. detecting hammer position at multiple proximity positions in a loop won't work well if we rely on ADC-conversion because most programmable controllers such as Arduino/Teensy, etc. have only a few ADC chips and a single conversion takes microseconds which would cumulatively lead to inability to scan the entire keyboard without introducing huge latency and without missing the very fast hammer motion.)


