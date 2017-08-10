# Magnet Simulation in Unity

## Motivation
I always thought magnets were pretty cool. Last year I tried to find some kind magnet simulation code but I could not find anything. So I decided to make my own magnet simulation in Unity.

https://www.youtube.com/watch?v=LzGUpQDTq7A

I started with trying to get the most basic understanding of electromagnetism. I experimented with several equations from Wikipedia and chose the Gilbert Model to calculate force between two magnetic poles.

So this is the equation that makes everything work.

![](https://steemitimages.com/DQmazbHJojCUuhUAjRVkuiXxknEkNDWweHymuBMVDyPoUY8/image.png)

https://en.wikipedia.org/wiki/Force_between_magnets#Force_between_two_magnetic_poles


Then the code translation of that formula.

    Vector3 CalculateGilbertForce(Magnet magnet1, Magnet magnet2)
    {
        var m1 = magnet1.transform.position;
        var m2 = magnet2.transform.position;
        var r = m2 - m1;
        var dist = r.magnitude;
        var part0 = Permeability * magnet1.MagnetForce * magnet2.MagnetForce;
        var part1 = 4 * Mathf.PI * dist;

        var f = (part0 / part1);

        if (magnet1.MagneticPole == magnet2.MagneticPole)
          f = -f;

        return f * r.normalized;
    }

To start with any magnetic simulation you will need a GameObject with the MagnetWorld script attached. This script does all of the magnetic simulation.

## Magnet World Settings
You will probably not need to change these.

**Permeability**: Permeability of free space. https://en.wikipedia.org/wiki/Permeability_(electromagnetism)
**Max Force**: This is a cap on the amount of force that can be applied during one frame.

# Simulation
## FixedUpdate
First all of the objects of type `Magnet` are gathered. Then the magnetic force is calculated and applied to each Magnetic pole.

## Magnet
Magnet Force: The strength of the magnet.
Magnetic Pole: One of the two ends of the magnet. Similar pole values repel each other, opposites attract.
RigidBody: The GameObject that forces will be applied to.

## Simple Setup
GameObjects and their components
	Magnet Object - RigidBody
		Pole - Magnet with Magnetic Pole set to North
		Pole - Magnet with Magnetic Pole set to South


So to setup the most basic magnet scene:
	1. Add GameObject with MagnetWorld component.
	2. Add two Magnet objects as described above.

Done.

### Use Cases
There are many more cool ways to use electromagnetism. Here are some examples:

## Mega Magnet

![](https://steemitimages.com/DQmZd2rQu8uXvZsbMDgdJgaavV736BPTHtitAF8fGaMXBs1/image.png)

https://youtu.be/xGE5A2ZcuJ0

I toy around with the power of a magnet that contains a bunch of South magnetic poles and an array of magnets with only North magnetic poles.


## Magnetic Levitation

![](https://steemitimages.com/DQmcQ7BmvnV3b6x7fsbFCqdrCgH7WZiVqs9D5p6p7Hzk77b/image.png)

https://www.youtube.com/watch?v=mbHOFR1ceFA


Having both North and South poles on the same object is not required to make magnets work in this simulation. For example in the Magnetic Levitation example it is set up like this:

![MagLevSetup.PNG](https://steemitimages.com/DQmaxjBmtDp7eBnFj1JwPDJwTBPihutnTwhBYqqF8gqL5EG/MagLevSetup.PNG)

The blue spheres represent North magnetic poles. Both the MagnetTrain and MagnetFloor objects are composed of North magnetic poles. The MagnetFloor magnets move in sync with the MagnetTrain. This enables the MagnetTrain to always be levitated above the floor. To move the MagnetTrain when pressing the arrow keys some of the Magnets are slightly weakened causing an imbalance of force being applied to the MagnetTrain.

## Iron Filings

![IronFilings.PNG](https://steemitimages.com/DQmeGBBzgnKuj3J6tNpt7zj2WPRvDyGxJWfqhTpzL4yDxg2/IronFilings.PNG)

![MagneticField.svg](https://steemitimages.com/DQmaBMm6TWEhbM5g8YrSCZkPpzcuWoME81sk2u7f4bHeq2k/MagneticField.svg)

This is a test to compare what the magnet field looks like using this simulation vs what a real world magnetic field looks like. Looks pretty close to me. The 'iron filings' are basically magnets set up with a rigid body that can only rotate. Translation is disabled.

Feedback and comments are greatly appreciated but please add an insult to make it interesting. Here is a template to help out:

[comment body] and go fuck yourself.

Example:

I love magnets and go fuck yourself.

kubiak@protonmail.com


