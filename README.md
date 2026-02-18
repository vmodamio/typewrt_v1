# typewrt_v1
Typewrt is a personal project dedicated to produce the perfect typewriter device. As it is intended to
suit all my needs (and dreams), I did not spare costs. I just wanted a device that did not feel like a 
DIY device to showcase. I wanted an 'instrument' that feels perfect for writing fiction. So this 
project didn't start with a 'distraction free' propaganda in mind, but just simply as a device that 
one truly feel better for the matter of writing and editing fiction.  

A compact device I can write in my desk, solid and heavy. But at the same time something I 
could fit in a small case and take to a coffee shop. First and foremost, something I could take over 
a laptop any time. This, among the following criteria, implying a much better battery life. Much, much
better to the point of forgetting you are typing in an electrical device.

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
 and quality of the 'instrument'. That in my opinion is unsurpassable. I dont know however, if I would take more photos
if my camera were a Leica. Maybe the joy of using it would be perennial, maybe those more photos wouldnt be any better.

I wanted something like that for my typewriter. Not that pretentious, but having that feeling when typing, of something
that is precise and comfortable. Exquisite to type in. So the typewriter should have a mechanical keyboard loaded with
tactile switches. Like the double-scape of the piano keys. Something you depress until you feel a resistance, and then 
the execution of the full stroke registering the event. At the same time I
wanted it completely silent. That is my preference, simply because I dont want
to have a minuscule remorse when typing in a public space (afecting my writing). Or even worse, when
being alone with my mind blank and hearing the clicks of those keys muted. 

I bought a keyboard PCB, that included a ATmega microcontroller and connected via USB. Using it with a raspberry is 
plug and play. I bought as well a beautiful bronze plate that I would have mounted in the final enclosure, together 
with the display. A 60% ISO keyboard layout, and the PCB being fully programable with VIA firmware. It was very 
promissing. The 60% keyboard layout suited all my needs. Arrows, home, insert and end keys are something you dont use 
if you write in VIM text editor. And even though, the VIA firmware allows you to program several 'layers' on your 
keyboard, where you can allocate the keys you want as combination of keys, or switching to a layer beforehand.

The big problem with USB HID (human interface device) is that it uses polling to get the user events. The microcontroller
is scanning at 20-100 MHz the whole keyboard matrix, continuously. That would be efficient for a gamer with a desktop PC,
but the power consumption is extreme. The PCB I bought was draining around 0.5A! That was about 4 times more than the 
computer itself (by the time I was using a Raspberry SBC). 

So I started to look for alternatives. The next would be to go with a wireless bluetooth (BLE). Even if the keyboard
and display and computer end in the same enclosure. The BLE keyboards drain much less power than USB. Something around 
30-50 mA I would guess.

I thought, maybe there are other PCB designs with power consumption in mind. I couldnt find any PCB for comertial mechanical
keyboards with that specs. But I did find lot of microcontrollers with application notes for ultra-low power. This document, 
[Implementing an ultra-low power keypad...](https://www.ti.com/lit/an/slaa139a/slaa139a.pdf?ts=1771453180695&ref_url=https%253A%252F%252Fwww.google.com%252F) from Texas Instruments was my starting point.

So I decided to buy a microcontroller evaluation board, with something minimal, with enough pin counts to scan the 
whole matrix, and capabilities to go to deep-sleep power states. The microcontroller is a STM32 Nucleo board, and I programed
it bare-bone (manipulating registers). It is working with interrupts so it doesnt require the computer to pull data. 
Everytime a key-event is registered, the event is sent via uart to the computer. 
The switches then would be mounted in a new plate, but then wired below, without PCB, and finally the whole matrix connected 
to the small microcontroller board (that fits below the keyboard). The advantage of that is that I could wire it as I wanted. 
So I arranged the keys in a 7 cols by 9 rows matrix, reducing the number of pins to 16 (for scanning 62 keys). 

Programing the keyboard was a long long odissey. I managed to have it working quickly, but not functinal. Key-events where 
appearing sometimes randomly. The board didnt go to sleep, or went to sleep to never wake up again. Then two key
combinations were not registered, or the combination was missinterpretted by the scaning algorithm.

Ended implementing a [debounce/denoise algorithm that found very elegant](https://sev.dev/hardware/better-debounce-algorithms/). 
Then, the interrupt mechanism was modified to be able to still register keys while pressing others. Once a key triggers the event,
the keyboard would still scan the whole matrix for more events, during a small period of few seconds, going to sleep after timeout.
New registered events reseting the timer to zero.

### Board (SBC) and OS
The Raspberry Pi zero W was a evry nice board to start with. The documentation is superb and the amount of examples and external
support also. But they are not the most efficient and curated boards regarding power consumption. The Board consumed around 120 mA
or 0.6W of power. I changed it for a similar format board from Radxa. A Rock-S0 board, with a quad core Arm-cortex A35, 512 MB of DDR3 
ram, and 8 GB of EMMC memory. The cortex A35 is a step forward in efficiency and low power, and the board is very nice in the sense
it does not include peripherals that I dont need. 
The challenge was to install the display linux kernel driver and the keyboard driver I already had for the raspberry. Because the 
documentation from Radxa is very scarce. I managed to install an Armbian linux on the board (Armbian is the linux distribution that 
support more embedded systems from Arm), and with lot of private feedback from Radxa I managed to make my drivers work in the new board.

Later I optimized it to reduce power at maximum. I reduced the cpu clock frequency, dissable two of the cores, reduce the ram frequency
as well, and dissable HDMI, USB and Ethernet peripherals. 

### Battery, UPS board and connectors


### Enclosure
Design, ergonomics, 3D print, gluing, annealing... inserts,
