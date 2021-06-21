The core component of pypilot is the pypilot software. The pypilot software is open source, free software, and for people who know how to deal with it: that source is available at https://github.com/pypilot. The readme file at github has all the information that you need to install the software from source. But this is very technical stuff. If you are not familiar with open-source software, this is not for you.

### Distributions
To make open-source software available to a wider public, ready-made distributions are made out of it. Two Pypilot distributions will be discussed in this workbook:
* OpenPlotter image, and
* TinyPilot image.

**OpenPlotter** is an elaborate marine navigation system, based on raspberry pi hardware and various open-source, free software components. Pypilot is one of them. You can download the system from the  openplotter download site as an _image_, put it on an SD card, stick it in a raspberry pi and you have all the software components at your fingertips. 

**Tinypilot** is also an image-based pypilot distribution, that sits well on a small, dedicated raspberry pi; dedicated to and highly optimized for just pypilot. It can be downloaded from the pypilot.org site. It is slightly more technical to roll out, as it is intended to run with a specially made keyboard and display unit, although you could do without. The good news is, that you can buy a ready-made tinypilot at the pypilot store, including hardware and software.

> **Image** - An 'image' is the contents of a complete disk. Rather than first installing an operating system like linux on a computer, and then installing all other pieces of software on it and then configuring them to work together: an image is the contents of a disk that has all those components preinstalled on it. All you need to do is download the image (typically one large file) and burn it onto a disk. If that sounds difficult – it isn’t: you can be up and running in half an hour.

Pypilot runs on linux, but it was developed on Raspbian, and most implementations run on raspberries pi3 or pi4. Tinypilot is intended to run on a pizero-w; the image has a special lightweight linux on it called tinycore.

![image](https://user-images.githubusercontent.com/17980560/110975408-ffb33b00-835f-11eb-90db-61a957d217cc.png)

Another free, open-source distribution that includes pypilot is **LysMarine BBN (Bare Boat Necessities) Edition** https://github.com/bareboat-necessities/lysmarine_gen.

***
[The hardware component >>>](The-hardware-component)