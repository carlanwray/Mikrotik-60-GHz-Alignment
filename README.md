#Mikrotik 60 GHz nRAY or LHG 60/G Alignment Instructions

Lubricate all of the screws on the solidMOUNT with something like Zoom Spout turbine oil, then tighten the locking screws until you can move it without racking the mount. Keep the center locking screw pretty tight so the mount is not sloppy. By doing this, you will save a lot of trouble later when fine-tuning and locking down the mount.

Align units with tx-sector=auto (default setting)

Once connected get as close to center sector* (below) then change tx-sector to center:

/int w6 set 0 tx-sector=xx
*(Center sector: nRAY xx=31 LHG 60/G xx=36)

Start bandwidth test at 500 mbps UDP from one end:

/tool bandwidth-test direction=both remote-tx-speed=500M local-tx-speed=500M protocol=udp address=REMOTE IP user=admin password=PASSWORD

Then go into alignment mode in another teminal:

/int w60g align 0

Continue to peak RSSI while also watching tx-mcs and tx-packet-error-rate. Use the 10 second RSSI average to gauge your progress. TAKE YOUR TIME!
Each face on the solidMOUNT adjustment bolt = ~0.45 degrees in the vertical axis and is roughly the same for the horizontal. The width of a single beam is only 0.2 deg. (YES ITS THAT SENSITIVE!)

When done set your tx-sector back to auto:

/int w6 set 0 tx-sector=auto

Below is the beam sector patern for each unit. align and monitor tx-sector-info are basing their left/right up/down directions on this chart i.e. tx sector = 48 then dish needs to go left 0.2 deg (one sector from 48 to 49) and up 0.4 deg (two sectors from 49 to 31)

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
