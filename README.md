# Mikrotik 60 GHz nRAY or LHG 60/G Alignment Instructions

## Mount tips and preliminary alignment info
Lubricate all of the screws, washers, and even the pivoting bracket faces on the solidMOUNT with something like Zoom Spout turbine oil[^1], then tighten the locking screws until you can move it without racking the mount. Keep the center locking screws tight enough that the mount is not sloppy. By doing this, you will save a lot of trouble later when fine-tuning and locking down the mount. Oh, and do yourself a favor, use a 10 mm wrench for alignment adjustment. Your Leatherman will not give you the precision you need.

Use either the sight tube or scope mount on the nRAY or a scope mounted to a level clamped to the LHG 60G to initially align the radio. *For longer links the scope is required to prevent insanity.* (NOTE: The nRAY scope mount is floppy, make sure it is resting solidly on the top mounting area, and don't touch it with your face while aligning!)

Whenever you are aligning it seems like it works better if you are pushing data across the link. Plus it gives you another metric to watch as you align. I just do a Bandwidth Test from radio to radio using these settings:

`/tool bandwidth-test direction=both protocol=udp address=REMOTE IP user=admin password=PASSWORD`

(you could also add `remote-tx-speed=500M local-tx-speed=500M` if you are having trouble monitoring the other side of the link but these units have fast enough CPUs to run the link full)

## Initial alignment

Now begin to fine-tune the alignment starting on one side of the link with `tx-sector=auto` (default setting). I do the initial alignment, from a terminal, with `/int w60g monitor 0`, not the alignment tool. (NOTE: It seems like the radios scan the beam sectors more quickly initially and then slow down once the best sector is selected if you find that the monitor output above is not changing as fluidly as you would like you can switch to align but it is so jittery that it's almost useless until you set a tx-sector. Also, the shorter the link the more difficult it is to align from my experience)

Once connected get `tx-sector` as close to center sector* as you can using the direction references from the `monitor` `tx-sector-info:` line above. (see notes about beam sector at the bottom)

## Alignment fine-tuning

Now change `tx-sector` to center:

`/int w6 set 0 tx-sector=xx`  
*(Center sector: nRAY xx=31 LHG 60/G xx=36)  
NOTE: I find it best to set the `tx-sector` on the other side of the link to the sector it automatically finds after preliminary alignment so that isn't messing with your local readings. Just make sure you set it back before adjusting that side.

Make sure you are still pushing data across the link with a Bandwidth Test.

Then go into alignment mode in another terminal:

`/int w60g align 0`

Continue to peak RSSI while also watching tx-mcs and tx-packet-error-rate. Use the 10 second RSSI average to gauge your progress. TAKE YOUR TIME!
Each face on the solidMOUNT adjustment bolt = ~0.45 degrees in the vertical axis and is roughly the same for the horizontal. The width of a single beam is only 0.2 to 0.8 deg. (YES ITS THAT SENSITIVE!)

## Second side of the link
Now repeat this process for the other side of the link starting with [Initial Alignment](https://github.com/carlanwray/Mikrotik-60-GHz-Alignment#initial-alignment) above.

When completely done set your tx-sector back to auto:

`/int w6 set 0 tx-sector=auto`

NOTE: You may need to do redo the alignment a second time on the first side after fine-tuning the second side.

## Notes about beam sectors

Below is the beam sector pattern for each unit. align and monitor tx-sector-info are basing their left/right up/down directions on this chart i.e. LHG 60G tx sector = 53 then dish needs to go right 0.6 deg (one sector from 53 to 52) and up 1 deg (two sectors from 52 to 36) (the vertical is inverted as near as I can tell. Meaning if the sector is below center in the chart the antenna needs to be aimed up. But in the horizontal, if the sector is right of center then the antenna needs to be aimed right. It's very confusing and I will update this if I find otherwise)

nRay 60G sector pattern
00 01 02 03 04 05 06 07 08  
09 10 11 12 13 14 15 16 17  
18 19 20 21 22 23 24 25 26  
27 28 29 30 *31* 32 33 34 35  
36 37 38 39 40 41 42 43 44  
45 46 47 48 49 50 51 52 53  
54 55 56 57 58 59 60 61 62  
(Center Sector 31)  

LHG 60G sector pattern  
00 01 02 03 04 05 06 07  
08 09 10 11 12 13 14 15  
16 17 18 19 20 21 22 23  
24 25 26 *27* *28* 29 30 31  
32 33 34 *35* *36* 37 38 39  
40 41 42 43 44 45 46 47  
48 49 50 51 52 53 54 55  
56 57 58 59 60 61 62 63  
(Center Sector 27,28,35,36. Alignment tool refrences 36 as center I belive)  
  
 ## Sources
[Manual:Interface/W60G](https://wiki.mikrotik.com/wiki/Manual:Interface/W60G) especially [Device RF Characteristics](https://wiki.mikrotik.com/wiki/Manual:Interface/W60G#Device_RF_characteristics)  
[nRAY Faulty or not](https://forum.mikrotik.com/viewtopic.php?f=7&t=170578)  
[WG60 Alignment - Can you really get it Centered](https://forum.mikrotik.com/viewtopic.php?t=170597)  
[LHG 60G Alignment question](https://forum.mikrotik.com/viewtopic.php?t=179949)  
[LHG 60G experience](https://forum.mikrotik.com/viewtopic.php?f=7&t=133374)  
[LHG 60G experience 2](https://forum.mikrotik.com/viewtopic.php?p=812183)  


[^1]: I have considered using locktite, a silicone or even a graphite lubricant but so far the turbine oil I keep on my truck has not caused any issues. I will update this if it ever becomes a problem. And I wouldn't do this if it didn't make a significant difference on dialing the link in.
