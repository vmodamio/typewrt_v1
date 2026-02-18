# typewrt_v1
Typewrt is a personal project dedicated to produce the perfect typewriter device. As it is intended to
suit all my needs (and dreams), I did not spare costs. I just wanted a device that did not feel like a 
DIY device to showcase. I wanted an 'instrument' that feels perfect for writing fiction. So this 
project didn't start with a 'distraction free' propaganda in mind, but just simply as a device that 
one truly feel better for the matter of writing and editing fiction.  

A compact device I can write in my desk, solid and heavy. But at the same time something I 
could fit in a small case and take to a coffee shop. First and foremost, something I could take over 
a laptop any time. The criteria? 

## Criteria

### Display
The screen, to start with. The LCD display literally worns out my eyes. The visibility is poor in daylight, 
and the power consumption is huge. That is the main reason I started this, together with the boom of the eink 
displays, and the discovery of the comertial FreeWrite. 

Very quick I bough myself a beautiful Waveshare 7.9-inch eink display with HD resolution and many shades of
grey. The model came with a hat that implemented propietary waveforms, promoting a fast refreshing rate and 
the possibility of partial updates. And near zero power consumption (wait). So I wired all the stuff, put 
it on a raspberry Pi and prepared a linux kernel driver to use it 'as I type'. 
The refreshing rate was so slow (300 ms) that writing was completely unpractical. Sure, one could deal with
the fact that whole words appeared at once. But making a typo and start hitting the backspace was a turn off. 
The power consumption was huge. Not as bad as an LCD, but definitely not something for a battery driven device.
At the maximum refreshing rate, and taking into account the size and resolution, the display was very power 
hungry. Not to say the 'artifacts' and the noise it quickly appeared in the screen with that refreshing rates.
And that beautiful FreeWrite that cost over 500 euros? How does a dedicated writing device justify the price
then! 

After many days of researching, I discovered a reflective (no backlight), ultra-low power display from SHARP 
called memory-in-pixel. The largest model was only 4.4 inches. They were monochromatic, and the resolution, 
only one quart of VGA (320 x240). The display, as seen in many posts on the web look beautiful. So contrasty 
that almost look like a epaper. The technology behind? It has a one bit memory per pixel that keeps the 
image (it is black or white), so one only need to update the pixels that change. The flipping of the bias for 
the liquid crystals (refreshing the LCD) consumes also very little power. The screen is rated at 50 uA when 
idle, and 600 uA when refreshing. The refreshing rate being like a normal LCD (fast).

I felt inmediatly in love with this screen. You can see this [marvel of gaming toy](https://play.date/) employing
the display with fantastic animations. The project started to get shape around it. A monochromatic
screen. What could be better for just writing fiction in a terminal (of course not ideal for your C++ highlighting). 
The size was a concern. But a VGA font (8x16) would give 15 lines of text with 40 characters. The resolution was 
also a compromise. Instead of that beautifully sharp typefaces from the HD eink display, I'm coming back to the 
terminal font appearance (more on that later). 

The last issue with the diplay was that no one provided a breakboard to use the 4.4 inches model. Adafruit had a 
breakboard for the smaller ones though, but going smaller than 4.4 inches was too far from a perfect typewriter. 
I just needed to learn how to make a PCB.

### Keyboard
If you like photography and compare, for example, the quality of the imagery produced with a Leica and a top notch
Sony camera, I promise you wont see any difference. Maybe the photographer with the Leica is more profesional, or 
his/her style of photography pairs better with a compact rangefinder. Or maybe there is in fact, that 'distraction-free' 
argument playing a role too, or maybe not to any of that. But if you have ever taken a Leica M in your hands and focus 
through the rangefinder, spining the objetive to get the focus, and press the shutter...you can feel the precission
 and quality of the 'instrument'. That in my opinion is unsurpassable. 

I wanted something like that for my typewriter. Not that pretentious, but having that feeling when typing, of something
that is precise and comfortable. Exquisite to type in. So the typewriter should have a mechanical keyboard loaded with
tactile switches. And at the same time completely silent. That is my preference, simply because I dont want to 
have a minuscule remorse when typing in a public space. Or even worse, when being alone with my mind blank and hearing 
the clicks of those keys muted. 
  

