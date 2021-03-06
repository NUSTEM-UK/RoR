# ROR
Robot Orchestra Returns

 ## Work in progress!

This isn't yet intended for use, though the `master` branch should typically work... if you have the rest of the system set up right. Which isn't documented yet. So... yeah, it's not really usable at the moment.

You'll also notice that the code is scrappy, with libraries and support files existing in multiple places throughout the file tree.

TL:DR; you may be eaten by a grue.

## Current status

[![Robotic Glockenspiel](http://img.youtube.com/vi/-eCzMi-BVEc/0.jpg)](http://www.youtube.com/watch?v=-eCzMi-BVEc "Jingle Bells")

As of Christmas 2017: robot online! Here's our home-made copper pipe glockenspiel playing requests from Twitter. Anything sent to [@NUSTEMxmas](https://twitter.com/@NUSTEMxmas) will be interpreted as a request for a festive song, which is then played back in the NUSTEM office. There's an [outline of this particular implementation on the NUSTEM blog](https://nustem.uk/xmas2017).

## Outline

Our Robot Orchestra consists of a collection of instruments, each played by a microcontroller. These are typically Wemos D1 mini-based platforms, although a Pi/Python client also now exists. Every player connects to a common network over wifi, and subscribes to an MQTT topic (`orchestra/playset`) which broadcasts note commands. Each player is also assigned (in software configuration, or in the case of Wemos-based players at runtime via patch cables) a channel number, which determines which note from the playset is responded to.

Possible system controllers are:
* An Adafruit Hella UNTZtrument-based hardware control surface, which is tactile and pretty but constrained to 8 channels / 16 beat loops.
* A (GUIZero-based) software implementation of that interface, which is configurable to a practical maxium of about 64 beat sequences before the GUI buttons are small enough to make your eyes go funny.
* An RTTTL ringtone interpreter, which is itself commanded over MQTT (see below).

The system continues to evolve as we head towards public performances in 2018.

## Directories

* `controller/` : Python-based orchestra controller with a GUIZero interface.
* `controller-ringtone/` : Python-based controller which parses an RTTTL ringtone string (itself passed in via MQTT)
* `experiments/` : Steps along the way, trying to make various components work.
    * `piano_pi/` : software-based playback, heavily based on [Pimoroni's Piano HAT](https://github.com/pimoroni/Piano-HAT) code. Used for testing RTTTL parsing and playback. RTTTL parsing is drawn from dhylands' [upy-rttl](https://github.com/dhylands/upy-rtttl).
* `glock/` : Handles Twitter requests.
* `legacy/` : Reference code pulled across from our ealier [Robot Orchestra repo](https://github.com/NUSTEM-UK/Robot-Orchestra).
    * `trellis/` : Python-based orchestra controller using the Adafruit Hella UNTZtrument control surface, over GPIO.
* `network/` : MQTT traffic monitors, so we can see what's going ~~wrong~~ on with our network messaging.
* `player/` : Wemos D1-based player client.
* `player-pi/` : Python-based player client for Raspberry Pi GPIO. Uses GPIOZero, with `pigpio`-based servos (and hence can handle at least 8 channels from a single Pi).

 ---

## GUIzero & Mac notes
The Controller relies on prerelease [GUIzero](https://github.com/lawsie/guizero) additions. ~~Today, the forthcoming GUIzero 0.4 is not working (with Waffle objects). Instead, I did a straight
`sudo pip3 install guizero`, then integrated the clickable waffle pull request by hand (!). That is: amend `/usr/local/lib/python3.6/site-packages/guizero/Waffle.py` with the changes from [this pull request](https://github.com/lawsie/guizero/pull/28/files). This is not a pretty way of working. However, hopefully the issue will go away with the release of GUIzero 0.4.~~

As of 2017-11-07, GUIzero `version-0.4` branch is working correctly (thanks [@codeboom](https://twitter.com/codeboom)!). To install:

    sudo pip3 install --upgrade git+https://github.com/lawsie/guizero.git@version-0.4

...but note that your previous GUIzero projects may need changes. Oh, snap - this is why people do Python development in [virtualenv](https://virtualenv.pypa.io/en/stable/). I get it now.

---

## Things we've learned along the way

1. Why we should be working in virtualenv.
2. Dual-homing a Raspberry Pi on two wifi networks, with routing to each simultaneously.
3. Successfully connecting a Pi to our (odd) university wifi network, as a side-effect of (2.)
4. Parsing and collation of RTTTL ringtones, which gives us a ready library for the next stage of the project.
5. Pygame sound generation, which gives us a way of composing for the orchestra without first building a set of instruments.
6. Decent practice with Python dictionaries, of which we've previously been a bit scared.
7. First steps into GUIZero, which is shaping up to be extremely useful for this sort of hack.
8. Python timers, though we're not yet using threads particularly.
9. That festive tunes played by bashing bits of copper pipe with servos get old, fast.