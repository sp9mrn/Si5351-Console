Si5351/A Console
================

Makes most Si5351/A functions available in command-line form.  This is very
useful during the bring-up/debug of home-brew radio projects.  Once things
are working properly some rig-specific firmware is generally needed.

Bruce MacKinnon KC1FSZ
31-December-2017

Usage Notes
===========

This is normally used with the serial monitor component of the Arduino IDE.  
However, any terminal program can be used. Be sure to set your terminal program
to 9600/8/N/1 and define the EOL character as "newline" (ie: \n, ASCII 10).

The Si5351 clock is set to this value:

    Display Frequency * Multiplier + Offset

Illustration For Superhet Developers
------------------------------------
An illustrative combination shows how this can be helpful when developing
superhets.  Imagine we're creating a 40m rig (7MHz, LSB) with an IF of 12MHz. A
common design would be to create a VFO at 5MHz and a BFO at 12MHz.  But also
imagine that your IF filter is a 2 KCs low, so the best BFO should actually be
at 11998000 Hz. The math can get a bit tricky because you need to subtract the
desired "display" frequency from the 12MHz IF (actually 11998000Hz) to create
the VFO.  Rather than keeping all of this in your head, you can configure the
Multiplier to be -1 and the Offset to be 11998000.  When you set the display
frequency to 7255000 the the output clock is actually set to:

      7255000 * -1 + 11998000 = 4743000

Even better, when you step "up" the display frequency, this actually steps
down the VFO (as expected).  

Another Illustration
--------------------
This illustration shows a helpful way to experiment with the optimal
BFO setting when you don't know exactly how your IF filter is working.
If can be helpful to step up/down the BFO while keeping the display frequency
constant.  You might do this by listening to a side-band conversation and
adjusting the BFO for the best audio quality.

But when you shift the BFO you also need to shift the VFO by the same
amount.  The math here can be tricky as well.  

Step mode "1" causes the display frequency of the selected clock to step
up or down by the configured step size.  Step mode "2" causes the *offset* 
of the selected clock to step up/down while keeping the display frequency
constant.

So a typical configuration for filter optimization would put the VFO clock
into step mode 2 and the BFO clock into step mode 1.  That way pressing +/-
will move up/down the BFO and the VFO by the right amounts, while staying
on the same display frequency.  

Commands
========

    f0/f1/f2 <freq Hz>   Set CLK0/1/2 frequency
    o0/o1/o2 <freq Hz>   Set CLK0/1/2 offset
    m0/m1/m2 <mult>      Set CLK0/1/2 multiplier
    d0/d1/d2 <0|1|2|3>   Set CLK0/1/2 drive strength
    s0/s1/s2 <0|1>       Set CLK0/1/2 step-enabled
    co <correction>      Set correction in parts-per-billion
    ss <step Hz>         Set step size
    st                   Display Si53531/A status
    we                   Save all state to EEPROM
    re                   Load all state from EEPROM
    =                    Step up
    +                    Step up  
    -                    Step down
