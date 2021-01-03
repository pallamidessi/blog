---
layout: post
title: "Raspberry pi zero goes aquatic: Adventures in viscious environment"
date: 2021-01-03 00:00:01
categories: computer
featured_image: /images/cover.jpg
---

![intro](/images/2021-01-02-rpi-goes-aquatic/intro.jpg){: .center }

I've always wanted to do an aquarium pc build, where you submerge an entire
computer in mineral oil, which is non-conductive so suitable for electronics, but couldn't ever commit to it for various reasons[^0].

And like everyone else, I got a bunch of unused Raspberry Pis laying around, so I've
decided to give it a shot.  
Pretty straightforward so far, just need a container and
to buy some oil from Amazon. But at some point before committing to it someone asked me if it
would impact the WiFi signal... and I had no idea, so I measured it! 


## The setup 🏗️

For this project, I'm using a Rasberry Pi zero W. The Pi will be seated at the bottom of a 500ml jar,
1 meter away from the router as pictured here to make some consistent measurements:

{% assign filenames = "2021-01-02-rpi-goes-aquatic/setup_1.jpg,2021-01-02-rpi-goes-aquatic/setup_2.jpg,2021-01-02-rpi-goes-aquatic/setup_3.jpg" | split: "," %}
<div class ="image-gallery">
{% for name in filenames %}
    <div class="box">
    <a href="{{ site.imagesurl }}{{ name }}">
      <img src="{{ site.thumbsurl }}{{ name }} " alt="{{ name }}"  class="img-gallery" />
     </a>
    </div>
 {% endfor %}
</div>

We will first record the WiFi signal of the Pi by itself, then submerged in water(!) and then in
mineral oil.

We're going to just run `iwconfig` in a loop and extract the `Signal Level` and `Link
Quality` values to plot them. Signal Level is the strength of the signal in decibels
and Link Quality is the signal-to-noise ratio (i.e interferences).

Here the `sed` surgery/monstrosity I used:
<script src="https://gist.github.com/pallamidessi/7c4ddf453c974d39499ed97705033c93.js"></script>

And the `gnuplot` script to generate the plots:
<script src="https://gist.github.com/pallamidessi/d8322dc88078c5e1a66450630d606935.js"></script>

Doing a first measurement of the Pi by itself we get this:

![Clear plot](/images/2021-01-02-rpi-goes-aquatic/plot_clear.png){: .center }

As expected, pretty much perfect! 

## In water 💦

Obviously no direct contact is possible so I went with a hacky solution by wrapping the Pi in a
plastic bag and in the water it goes!

{% assign filenames = "2021-01-02-rpi-goes-aquatic/hack_plastic.jpg,2021-01-02-rpi-goes-aquatic/water.jpg" | split: "," %}
<div class ="image-gallery">
{% for name in filenames %}
    <div class="box">
    <a href="{{ site.imagesurl }}{{ name }}">
      <img src="{{ site.thumbsurl }}{{ name }} " alt="{{ name }}"  class="img-gallery" />
     </a>
    </div>
 {% endfor %}
</div>

After a few minutes we get those measurements:
![water plot](/images/2021-01-02-rpi-goes-aquatic/plot_water.png){: .center }

You can see here that the signal is severely affected: both the strength and noise
levels. You can a see a second peak in the data, this is when I changed the
orientation/position of the pi in the jar trying to maximize to the amount of water
between the router and the chip (and managed to do the opposite 🙈).

## In mineral oil 🛢️

After cleaning up everything and making sure everything was dry with a hairdryer I
did the same thing instead with mineral oil instead of water. I
wrapped the Pi again when measuring in case the wifi signal would be as bad as with water since cleaning up
electronics dipped in oil is _very_ time-consuming.

{% assign filenames = "2021-01-02-rpi-goes-aquatic/oil_1.jpg,2021-01-02-rpi-goes-aquatic/oil_2.jpg,2021-01-02-rpi-goes-aquatic/oil_3.jpg" | split: "," %}
<div class ="image-gallery">
{% for name in filenames %}
    <div class="box">
    <a href="{{ site.imagesurl }}{{ name }}">
      <img src="{{ site.thumbsurl }}{{ name }} " alt="{{ name }}"  class="img-gallery" />
     </a>
    </div>
 {% endfor %}
</div>

And here we get those plots 👀:
![oil plot](/images/2021-01-02-rpi-goes-aquatic/plot_oil.png){: .center }

Here, the result looks _much_ better. Almost as good as air!
Mineral oil is non-conductive thus will not absorb the radio frequencies of
WiFi, so rest will be converted as heat (but WiFi is *super* low-powered so the isn't
any risk, [100mW in most European
countries](https://git.kernel.org/pub/scm/linux/kernel/git/sforshee/wireless-regdb.git/tree/db.txt)).  
Water is also really [good at absorbing 2.4Ghz+ RF](https://en.wikipedia.org/wiki/Electromagnetic_absorption_by_water#Electronic_spectrum), which is the reason [microwaves also go with 2.4Ghz](https://en.wikipedia.org/wiki/2.4_GHz_radio_use#Microwave_oven)

## Final thoughts

Due to it reflection index, the Pi in the mineral oil, it appears quite distorted and
makes up for some trippy effect was put in a front of an interesting background,
like a tetris lamp:

{% assign filenames = "2021-01-02-rpi-goes-aquatic/outro_1.jpg,2021-01-02-rpi-goes-aquatic/outro_2.jpg" | split: "," %}
<div class ="image-gallery">
{% for name in filenames %}
    <div class="box">
    <a href="{{ site.imagesurl }}{{ name }}">
      <img src="{{ site.thumbsurl }}{{ name }} " alt="{{ name }}"  class="img-gallery" />
     </a>
    </div>
 {% endfor %}
</div>

I did record the temperature and have the raspberry run at 100% cpu for a while (>15 minutes) and since it doesn't heat up much anyway, with the 0.5L of oil it peaked at 44C and stayed there.

What have we learned ?

* Mineral oil is messy fun!
* Very slightly impacts WiFi, success! 
* Still no _real_ use for that Pi zero, but now it is pretty ✨


  
------

[^0]: Lazyness, cost, impossibility to move, makes a nightmarish mess if mishandled/broke but mostly lazyness
