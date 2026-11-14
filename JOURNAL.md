---
title: "RP2040 Devboard (Full GPIO Breakout)"
github: "https://github.com/Magnu-Shaak/Devboard-Rp2040-"
description: "Making a Custom rp2040 devboard. Hope to use this for my custom keyboard projects."
created_at: "2026-08-09"
total_time: "21h 6m"
---

# August 9, 2026: Starting the Schematic
<!-- fabricate:entry 1355 -->

So, in this project, I am basically just following the guide right now. I want to experiment latter, but right now I am following the tutorial and trying to understand why it works.

Initially when I started, I got confused right away. The symbol it used was very different from what I was using. Then I noticed, I did have those pins(for the 1v1 power circuit), but on the top right instead of the top left. After a quick google, I learned I could change it to the way the guide had it, but after checking I had the same pins, and only some of the pins were differently placed, and that was only by a bit. 

I also learned about decoupling capacitors. I have some (very basic) knowledge of how components work on a circuit(through a event in a competition), but I never got to Capacitors in my research. I knew how capacitors roughly worked, but I didn't understand how that would be very helpful. Turns out a lot. They can act as a mini rechargeable battery to be called up when needed. Anyways, I got to learn more about their use.

Finally (this is stupid) I got confused because of the reference screenshot from the datasheet. On there they have 7 0.1uF Capacitors, and the guide has 8. This is because for pins close enough together, you can basically "share" the same capacitor between them. Don't really understand it, but that's why the screenshot had things like "place this capacitor near pin xx". Anyways, the tutorial removes this for simplicity.

lapse link: https://lapse.hackclub.com/timelapse/nqKgU9OMMlHF
![image](https://fabricate.hackclub-assets.com/747f022a6196525e6adb92c34741c3fe72c6b4002b5719ab42cce6bc4a2cefdd/image-png/image.png)

**Total time spent: 1h 18m**

# August 11, 2026: Made Clock and Flash
<!-- fabricate:entry 1356 -->

As seen in the Image, I made the Clock and Flash Storage. The Clock is based on a 12 MHz Crystal. The Flash Memory is a W25Q128JSV (wow, that's a mouthful) chip. Of course, both use capacitors and resistors to smooth out the power signals. THs flash uses Quad SPI to communicate with the Flash Memory

As seen in the second image, I also labeled all the pins of the RP2040. I am still deciding what configuration to lay them out in. I want to have it spread out like the Pico zero, but with attention to where the power pin is. I hate when you have to route a trace to the complete other side to connect it to power. Will look into having multiple pins lead to the power pins. If it works, I won't have to run those messy traces across the controller like I had to with my macropod project.

lapse link: https://lapse.hackclub.com/timelapse/0T-CypomcJ9r

![image](https://fabricate.hackclub-assets.com/43009f5a7233e77faf90221fa3a1edaa4dd5ab053170e076595ee0329da68959/image-png/image.png)
![image](https://fabricate.hackclub-assets.com/9764724a14d3fe277211d569608ba02d377b77a05869b6efa26743e32e2c28ef/image-png/image.png)

**Total time spent: 1h 48m**

# August 11, 2026 (entry 2): Finished Schematic
<!-- fabricate:entry 1357 -->

Finished the Schematic. I looked around if splitting the power was possible. Didn't get conclusive answers.

I defaulted to the Pi-Pico Layout to base it off of. I still want to make the power pins on the bottom, possibly as a secondary power pin. I also have some unused pins I want to break out. About that, I plan to make a second layer to the side pins, and push the current pins further into the devboard pcb. You can't access them directly, but you can run a trace through the bottom underneath the devboard to connect it. This allows for more pins on the board, so I can fit the extra advanced testing pins, leftover GPIO pins, and power pins there as well.

I am also going to try to make this battery compatible. The guide recommended looking at the datasheet for it, so I'll start with that.

Edit: Found the datasheet. it referred to to a build sheet(Hardware design with RP2040: Using RP2040 microcontrollers to build boards and products). This one is short (37 pages) and should have the stuff I need for the battery compatibility.

lapse link: https://lapse.hackclub.com/timelapse/zoG4UY2OH_sw

![image](https://fabricate.hackclub-assets.com/724b58947e64dfac39742413359c46ffc558dae216d5b896fbc8a401af632cbb/image-png/image.png)

**Total time spent: 1h 12m**

# August 13, 2026: Finished Schematic and Started PCB
<!-- fabricate:entry 1358 -->

So I finished the Schematic. Compared to the one in the tutorial, I broke out all of the pins. I created a little inlet on my PCB. Here, the rest of the pins are broken out. For the left side, the most accessible one is the AD3/GPIO 29. This is the most useful pin because it's the only AD the left side will have access to. Apparently VBUS is pretty important too, but it can be split latter on to be used for multiple things, so that's why it's on the right side (which already has a VBUS at the top). It's meant to make accessing it easier, and I put it on the right because google things AD pins are more important than a VBUS. IDK, I just wanted to use all the pins and have never used AD pins before.

Above the VBUS in both sides is 3v3. A lot of electronics use this, so thats why its here. Also, using it doesn't block you from using the VBUS and AD3 pins, because those can be accesed directly from below. Therefore, this is the first pin you need to "go inside for". 
The other 3 GPIO pins are normal. These can easily be accessed from either below (with THT) or on the front (SMT), with good track placement. Oh, and as of now the blank pin is to GND (Easily accessible form the back) Might remove it if it prevents me from easily connecting the rp2040's internal pins to the break points.

lapse link: https://lapse.hackclub.com/timelapse/e_XY4ruCUwpo
![image](https://fabricate.hackclub-assets.com/31df4b97b0a1f5117eafa56b8e72d0a6229fab5d3b8c17313b3d3d2950cfb6cc/image-png/image.png)
![image](https://fabricate.hackclub-assets.com/413368af1ecf5c7b9d300ec98e540eda849202790f4b5213bd993761fd56217f/image-png/image.png)

**Total time spent: 1h 48m**

# August 14, 2026: Continued PCB
<!-- fabricate:entry 1359 -->

This session, I positioned my Power Decoupling Capacitors, Flash memory, and LDO IC (voltage regulator).

Right Away, I know that the top right corner, and top of the chip in general is going to be very busy. Decoupling capacitors work best when placed close to the pin, and there are 7 power pins there. As this is a hub, it's best to put the bigger 1F decoupling capacitors for both voltages (3v3 and 1v1) there as well. Id addition, there are the Flash Memory's pins, and the USB data pins as well.

BTW, turning off the fab and silks layers help so much when trying to make out where the components are.

lapse link:https://lapse.hackclub.com/timelapse/vmdCvdG25SpP
![image](https://fabricate.hackclub-assets.com/6ed152d2f7387a40b1b13b75dd8039396f4e3156e8061978b5cea0472fd5b5f9/image-png/image.png)

**Total time spent: 1h 6m**

# August 18, 2026: Lot of PCBing
<!-- fabricate:entry 1360 -->

Completed v1 of pcb. Many many tweaks were needed in the process (see lapse). 

Ground pour and IO pins were the most tedious. The pins on the Rp2040 SoC itself are like 1 trace wide. And then you need to route half up and the other half down. The right side was worse, each half had wonky correspondence. So the left (normal correspondence), the top gpio on the breakout conn. is the top one on the chip. But because I broke out all the pins, using the unused battery pin spots, those 3 pins had to go all the way to the top. Do those ones were red on the right, and since it was reverse order, they had to turn left, go though a via, and then right into thier breakout pin.

Come on. I just realized I could have just those 3 use vias, like i had the upper ones on the left side do. Might do it just for better routing.

![image](https://fabricate.hackclub-assets.com/5548a403f4c46d3e3d27798576dacba1ff82ff94a531798cc432aa8e7368ee48/image-png/image.png)

The USB-C Differential Pair was more confusing than annoying. My skew (difference in travell time) between them was so small, that adding a meander wouldn't work because of the minimum distance settings. I redid it and it worked. But then it got shifted and I had to redo it (off lapse, hence the extra 12 mins on this devlog). I learned how to lock down stuff after that.

General trace management was okay. Exept those small tweaks that take a lot of time, but you still need to do them. Worst feeling was when you finish, and realize there is a beter way to do it, and you HAVE to do it the other way for some reson)

Ohh, Don't forget the 7 errors + 58 warning that need to be checked and adressed! I fixed a few errors this session. Most of the warnings are about silkscreen/text getting cut off, IDC about that. A few others that I addressed were incomplete traces. These were mostly traces that got shifted, and the little piece between them was deleted. Annoying but normal. Whatever, I'm getting to them next time.

lapse links: 
https://lapse.hackclub.com/timelapse/NwnC0S5bfr6a
https://lapse.hackclub.com/timelapse/Lb5QEGojR7QR
https://lapse.hackclub.com/timelapse/BXc65_otpq53

**Total time spent: 6h 36m**

# August 18, 2026 (entry 2): Fixed Traces (right side)
<!-- fabricate:entry 1361 -->

Remember me 20 mins ago realizing there was an easier way to do that. Well, I loked at my PCB adn it was stupidly easy. I did it in 15 mins easy.

The one without vias going to the breakout is the new one.

![image](https://fabricate.hackclub-assets.com/dbc6b18e3064b0ef8b983c304ab1dcf4ed3efb826eb04ec18fe1888f21966b84/image-png/image.png)

![image](https://fabricate.hackclub-assets.com/2ae0a7748439ca4cdc944587da3ac60d4532dd16a2e51a7322fdf97cd0098966/image-png/image.png)

**Total time spent: 18m**

# August 21, 2026: Finished Making PCB
<!-- fabricate:entry 1362 -->

Been a while. In that time, finished the entire pcb. Tweaked a lot, moved a lot around. Main point is, the PCB is finished, all the violations are resolved, visual silkscreen is done. 

All I need to do is export gebers, and cross check the JLCPCB and PCBWAY quotes for the cheaper one. Got to learn how to format for pcbway for that (tutorial uses jlcpcb)

lapse links:
https://lapse.hackclub.com/timelapse/2IXxPACNh2U0
https://lapse.hackclub.com/timelapse/3YaIFr-HFUD9

![image](https://fabricate.hackclub-assets.com/1cee10e9360bc38b94b8ec6fb6a09832347982431012ed11ffd588b0d38e9296/image-png/image.png)

**Total time spent: 1h 54m**

# August 22, 2026: File Schenanagans
<!-- fabricate:entry 1363 -->

I despise Production.

First exporting the gerbers. Went okay. The guide never mentioned if it used Excelleron or X2 for the drill files (only showed for plot files with X2), so I spent time figuring that out. I don;t think it is a big deal, but its my first time ordering a pcb. One Ifound out (it was Excelleron) Ibegan to look up how to export the BOM and CPL files that the guide did ot mention. I was following the guide but using updated info from the JLC website.

Once I finish exporting the files, I still had to format them to JLCPCB's preference. But then I began to wonder, why does the new guidlines not have a quantities column. And then there are other columns  for info and whatnot that are in the guide's guidlines but not on the JLCPCB website. 

So I was confused, so I desided to do both, and just use whatever looked like the website understood beter. So I did. Was confusing because I had to rename and edit both files. And then halfway through the JLC's way told me to manually update something, and that caused hidden columns to appear in the Kicad BOM tool, so I restored a kicad backup... It was a whole mess.

Once I got it done for each, I took screenshots. They are the EXACT same to the JLC interpriter.

Anyways, I have my production files, and will check in with PCBway, which is sometimes cheaper once you count US tarrifs.


![image](https://fabricate.hackclub-assets.com/fdf3f5e3794f14717ece50d2e013a2738069ef6d9f774a7eba674f20a3706999/image-png/New.png)![image](https://fabricate.hackclub-assets.com/6ea25eec4e7a85d91923fa1659d4424cf25829d91d38abe3eb9cd2dfbf75f7b1/image-png/Old.png)

**Total time spent: 3h**

# August 24, 2026: Submitting for Review
<!-- fabricate:entry 1366 -->

Note: this was when I submitted to Macondo

Actually Submitting for review. When I went to see if I could decrease the Assembled PCB count last time, the order had expired. I began to remake it, but had to go somewhere and the pressure got to me. When I came back I remade the BOM.

Also, I corrected for the JLC import errors. JLC has a different rotation scheme, so I manually edited the CSV and changed it. Also, I added the JLC Part Numbers to the BOM, so recreat-ers (if there are any, I don't think there will be) don't have to fidle with the rotation or moving the USB port back to alignment.

Anyways, the assembled pcb cunt is reduced to 2. I checked, and there ae no basic part replacements for what I need. The Final price is $67.35.

![image](https://fabricate.hackclub-assets.com/39bb2b250f24b8760e7bfafb4916ee9634b14fc55d2d82cbd371c0886525ac01/image-png/image.png)

**Total time spent: 2h**

# November 14, 2026: Regarding my Submitting to Fabricate
<!-- fabricate:entry 3003 -->

My previous Submission was on accident. I was just checking if my project had imported from Macondo. Many of the previous issues were easily fixable and I fixed them. The main Issue was logging progress. Macondo counted Hackatime hours and manual dev-logged hours as separate (i.e. dev-log hours **+** hackatime hours = total hours). I edited all my fabricate dev-logs and added hour **estimates**.
__
MAcondo also doesn't require a journal.md file on Github. Thats why I didn't have it, but I used the Sync to GitHub Feature to do that just now.
__
I ended my search for a standalone Wi-Fi chip. The chips for it are too big to fit on the devboard without breaking the pi-pico form factor. I want to make this devboard versatile, but I realized I can use a raspberry pi radio module 2 to add wifi for any project that needs them. I deleted my Wi-Fi chip related logs to avoid confusion. The main reason for my search was that I needed to differentiate my project from the kai perita tutorial. I'll address what I am going to do instead in my next devlog.
![](https://fabricate.hackclub-assets.com/e5ea950cec6eed5c0db5415fda0f3e7b92bf8374044119524cbf37df0d2f9cf9/image-png/image.png)

**Total time spent: 6m**
