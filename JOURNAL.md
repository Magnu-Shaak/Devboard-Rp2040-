---
title: "RP2040 Devboard (Full GPIO Breakout)"
github: "https://github.com/Magnu-Shaak/Devboard-Rp2040-"
description: "Making a Custom rp2040 devboard. Hope to use this for my custom keyboard project."
created_at: "2026-08-09"
total_time: "12h 18m"
---

# August 9, 2026: Starting the Schematic

So, in this project, I am basically just following the guide right now. I want to experiment latter, but right now I am following the tutorial and trying to understand why it works.

Initially when I started, I got confused right away. The symbol it used was very different from what I was using. Then I noticed, I did have those pins(for the 1v1 power circuit), but on the top right instead of the top left. After a quick google, I learned I could change it to the way the guide had it, but after checking I had the same pins, and only some of the pins were differently placed, and that was only by a bit. 

I also learned about decoupling capacitors. I have some (very basic) knowledge of how components work on a circuit(through a event in a competition), but I never got to Capacitors in my research. I knew how capacitors roughly worked, but I didn't understand how that would be very helpful. Turns out a lot. They can act as a mini rechargeable battery to be called up when needed. Anyways, I got to learn more about their use.

Finally (this is stupid) I got confused because of the reference screenshot from the datasheet. On there they have 7 0.1uF Capacitors, and the guide has 8. This is because for pins close enough together, you can basically "share" the same capacitor between them. Don't really understand it, but that's why the screenshot had things like "place this capacitor near pin xx". Anyways, the tutorial removes this for simplicity.

![image](https://fabricate.hackclub-assets.com/747f022a6196525e6adb92c34741c3fe72c6b4002b5719ab42cce6bc4a2cefdd/image-png/image.png)

**Total time spent: 6m**

# August 11, 2026: Made Clock and Flash

As seen in the Image, I made the Clock and Flash Storage. The Clock is based on a 12 MHz Crystal. The Flash Memory is a W25Q128JSV (wow, that's a mouthful) chip. Of course, both use capacitors and resistors to smooth out the power signals. THs flash uses Quad SPI to communicate with the Flash Memory

As seen in the second image, I also labeled all the pins of the RP2040. I am still deciding what configuration to lay them out in. I want to have it spread out like the Pico zero, but with attention to where the power pin is. I hate when you have to route a trace to the complete other side to connect it to power. Will look into having multiple pins lead to the power pins. If it works, I won't have to run those messy traces across the controller like I had to with my macropod project.

![image](https://fabricate.hackclub-assets.com/43009f5a7233e77faf90221fa3a1edaa4dd5ab053170e076595ee0329da68959/image-png/image.png)
![image](https://fabricate.hackclub-assets.com/9764724a14d3fe277211d569608ba02d377b77a05869b6efa26743e32e2c28ef/image-png/image.png)

**Total time spent: 0m**

# August 11, 2026: Finished Schematic

Finished the Schematic. I looked around if splitting the power was possible. Didn't get conclusive answers.

I defaulted to the Pi-Pico Layout to base it off of. I still want to make the power pins on the bottom, possibly as a secondary power pin. I also have some unused pins I want to break out. About that, I plan to make a second layer to the side pins, and push the current pins further into the devboard pcb. You can't access them directly, but you can run a trace through the bottom underneath the devboard to connect it. This allows for more pins on the board, so I can fit the extra advanced testing pins, leftover GPIO pins, and power pins there as well.

I am also going to try to make this battery compatible. The guide recommended looking at the datasheet for it, so I'll start with that.

Edit: Found the datasheet. it referred to to a build sheet(Hardware design with RP2040: Using RP2040 microcontrollers to build boards and products). This one is short (37 pages) and should have the stuff I need for the battery compatibility.

![image](https://fabricate.hackclub-assets.com/724b58947e64dfac39742413359c46ffc558dae216d5b896fbc8a401af632cbb/image-png/image.png)

**Total time spent: 0m**

# August 13, 2026: Finished Schematic and Started PCB

So I finished the Schematic. Compared to the one in the tutorial, I broke out all of the pins. I created a little inlet on my PCB. Here, the rest of the pins are broken out. For the left side, the most accessible one is the AD3/GPIO 29. This is the most useful pin because it's the only AD the left side will have access to. Apparently VBUS is pretty important too, but it can be split latter on to be used for multiple things, so that's why it's on the right side (which already has a VBUS at the top). It's meant to make accessing it easier, and I put it on the right because google things AD pins are more important than a VBUS. IDK, I just wanted to use all the pins and have never used AD pins before.

Above the VBUS in both sides is 3v3. A lot of electronics use this, so thats why its here. Also, using it doesn't block you from using the VBUS and AD3 pins, because those can be accesed directly from below. Therefore, this is the first pin you need to "go inside for". 
The other 3 GPIO pins are normal. These can easily be accessed from either below (with THT) or on the front (SMT), with good track placement. Oh, and as of now the blank pin is to GND (Easily accessible form the back) Might remove it if it prevents me from easily connecting the rp2040's internal pins to the break points.

![image](https://fabricate.hackclub-assets.com/31df4b97b0a1f5117eafa56b8e72d0a6229fab5d3b8c17313b3d3d2950cfb6cc/image-png/image.png)
![image](https://fabricate.hackclub-assets.com/413368af1ecf5c7b9d300ec98e540eda849202790f4b5213bd993761fd56217f/image-png/image.png)

**Total time spent: 0m**

# August 14, 2026: Continued PCB

This session, I positioned my Power Decoupling Capacitors, Flash memory, and LDO IC (voltage regulator).

Right Away, I know that the top right corner, and top of the chip in general is going to be very busy. Decoupling capacitors work best when placed close to the pin, and there are 7 power pins there. As this is a hub, it's best to put the bigger 1F decoupling capacitors for both voltages (3v3 and 1v1) there as well. Id addition, there are the Flash Memory's pins, and the USB data pins as well.

BTW, turning off the fab and silks layers help so much when trying to make out where the components are.

![image](https://fabricate.hackclub-assets.com/6ed152d2f7387a40b1b13b75dd8039396f4e3156e8061978b5cea0472fd5b5f9/image-png/image.png)

**Total time spent: 0m**

# August 18, 2026: Lot of PCBing

Completed v1 of pcb. Many many tweaks were needed in the process (see lapse). 

Ground pour and IO pins were the most tedious. The pins on the Rp2040 SoC itself are like 1 trace wide. And then you need to route half up and the other half down. The right side was worse, each half had wonky correspondence. So the left (normal correspondence), the top gpio on the breakout conn. is the top one on the chip. But because I broke out all the pins, using the unused battery pin spots, those 3 pins had to go all the way to the top. Do those ones were red on the right, and since it was reverse order, they had to turn left, go though a via, and then right into thier breakout pin.

Come on. I just realized I could have just those 3 use vias, like i had the upper ones on the left side do. Might do it just for better routing.

![image](https://fabricate.hackclub-assets.com/5548a403f4c46d3e3d27798576dacba1ff82ff94a531798cc432aa8e7368ee48/image-png/image.png)

The USB-C Differential Pair was more confusing than annoying. My skew (difference in travell time) between them was so small, that adding a meander wouldn't work because of the minimum distance settings. I redid it and it worked. But then it got shifted and I had to redo it (off lapse, hence the 12 mins on this devlog). I learned how to lock down stuff after that.

General trace management was okay. Exept those small tweaks that take a lot of time, but you still need to do them. Worst feeling was when you finish, and realize there is a beter way to do it, and you HAVE to do it the other way for some reson)

Ohh, Don't forget the 7 errors + 58 warning that need to be checked and adressed! I fixed a few errors this session. Most of the warnings are about silkscreen/text getting cut off, IDC about that. A few others that I addressed were incomplete traces. These were mostly traces that got shifted, and the little piece between them was deleted. Annoying but normal. Whatever, I'm getting to them next time.

**Total time spent: 12m**

# August 18, 2026: Fixed Traces (right side)

Remember me 20 mins ago realizing there was an easier way to do that. Well, I loked at my PCB adn it was stupidly easy. I did it in 15 mins easy.

The one without vias going to the breakout is the new one.

![image](https://fabricate.hackclub-assets.com/dbc6b18e3064b0ef8b983c304ab1dcf4ed3efb826eb04ec18fe1888f21966b84/image-png/image.png)

![image](https://fabricate.hackclub-assets.com/2ae0a7748439ca4cdc944587da3ac60d4532dd16a2e51a7322fdf97cd0098966/image-png/image.png)

**Total time spent: 18m**

# August 21, 2026: Finished Making PCB

Been a while. In that time, finished the entire pcb. Tweaked a lot, moved a lot around. Main point is, the PCB is finished, all the violations are resolved, visual silkscreen is done. 

All I need to do is export gebers, and cross check the JLCPCB and PCBWAY quotes for the cheaper one. Got to learn how to format for pcbway for that (tutorial uses jlcpcb)

![image](https://fabricate.hackclub-assets.com/1cee10e9360bc38b94b8ec6fb6a09832347982431012ed11ffd588b0d38e9296/image-png/image.png)

**Total time spent: 12m**

# August 22, 2026: File Schenanagans

I despise Production.

First exporting the gerbers. Went okay. The guide never mentioned if it used Excelleron or X2 for the drill files (only showed for plot files with X2), so I spent time figuring that out. I don;t think it is a big deal, but its my first time ordering a pcb. One Ifound out (it was Excelleron) Ibegan to look up how to export the BOM and CPL files that the guide did ot mention. I was following the guide but using updated info from the JLC website.

Once I finish exporting the files, I still had to format them to JLCPCB's preference. But then I began to wonder, why does the new guidlines not have a quantities column. And then there are other columns  for info and whatnot that are in the guide's guidlines but not on the JLCPCB website. 

So I was confused, so I desided to do both, and just use whatever looked like the website understood beter. So I did. Was confusing because I had to rename and edit both files. And then halfway through the JLC's way told me to manually update something, and that caused hidden columns to appear in the Kicad BOM tool, so I restored a kicad backup... It was a whole mess.

Once I got it done for each, I took screenshots. They are the EXACT same to the JLC interpriter.

Anyways, I have my production files, and will check in with PCBway, which is sometimes cheaper once you count US tarrifs.


![image](https://fabricate.hackclub-assets.com/fdf3f5e3794f14717ece50d2e013a2738069ef6d9f774a7eba674f20a3706999/image-png/New.png)![image](https://fabricate.hackclub-assets.com/6ea25eec4e7a85d91923fa1659d4424cf25829d91d38abe3eb9cd2dfbf75f7b1/image-png/Old.png)

**Total time spent: 3h**

# August 23, 2026: JLC Order

Completed Order on JLC PCB. Would be a normal thing, If I didn't have the August 31st Deadline. Because of that, I can't get normal Global shipping (8-13 days) for $7. MY cheapest is UPS Express(DDP) for an estimated $25.71!! Highway robbery, literally just for the highway (as in the quick shipping).

I will enter a nearby address and get the estimate. Will check pcbway, but doubt it will be much cheaper (only cheaper with global shipping)

![image](https://fabricate.hackclub-assets.com/f193d8f4af59b652d4c8127725219c9c9749ad6ed5d34a87bb096c12e0114b89/image-png/image.png)

**Total time spent: 2h 18m**

# August 23, 2026: Actual Cost

Went through the checkout process. Total comes out to $82.69 after tax, shipping, and a -$10 coupon
Edit: This is not what I ended off with, and the total is much less than that if you use a cheaper shipping option. I didn't because i was trying to submit to macondo, a month before the deadline.
![image](https://fabricate.hackclub-assets.com/dfb9824254e9acc3a1f16f255c8c07cbdd154b330d3d618264e300a60a92ae5f/image-png/image.png)

**Total time spent: 6m**

# August 24, 2026: Submitting for Review

Actually Submitting for review. When I went to see if I could decrease the Assembled PCB count last time, the order had expired. I began to remake it, but had to go somewhere and the pressure got to me. When I came back I remade the BOM.

Also, I corrected for the JLC import errors. JLC has a different rotation scheme, so I manually edited the CSV and changed it. Also, I added the JLC Part Numbers to the BOM, so recreat-ers (if there are any, I don't think there will be) don't have to fidle with the rotation or moving the USB port back to alignment.

Anyways, the assembled pcb cunt is reduced to 2. I checked, and there ae no basic part replacements for what I need. The Final price is $67.35.

![image](https://fabricate.hackclub-assets.com/39bb2b250f24b8760e7bfafb4916ee9634b14fc55d2d82cbd371c0886525ac01/image-png/image.png)

**Total time spent: 2h**

# August 31, 2026: Researched For Battery Power

My project got sent back because it didn't have enough changes from the guide. To be fair, thats valid. So I got to add stuff.

I looked into adding wifi capability. But I'm probably not going to use the wifi part for any builds forseeably, so I'm going to try to do something else instead.

My larger focus is for battery operation. There is a VSYS line for that. I am trying to see if I could break out that pin, and when I have sufficient power flowing through that, I use that instead of the main usb-c port. I want to be able to run it locally, of a big local battery wired directly, than through usb-c and not being able to reprogramm it at the same time.

Anyways, I started looking through the datasheet and relative docs, so here is a screenshot of the powering options.
![image](https://fabricate.hackclub-assets.com/6c8d96cb10a8eab3ccf8afe5f7a18cb5b5347d0759c6a11568cc06154842dad9/image-png/image.png)

**Total time spent: 1h**

# September 5, 2026: Switching WiFi chip

So I was researching about the wifi chips. I was using AI for this general search for actual WiFi chips (not boards) that connect to the rp2040. 

I was planning to use an infineon CYW 43439, which is the chip the Pico uses. However looking at the pi-pico schematics at the end of the datasheet shows that it uses a custom protocol with different pins. I then looked into connecting it with general SPI. But then I found out that the rp2040 uses a old SPI communication module that doesn't work well with modern chips like the infineon (the ARM PL022 SSP uses a old CSn line protocol, that causes modern spi devices to abort halfway through). I could use UART to connect it, but then I wouldn't get wifi.

Some more Google-Ai ing led me to the ESP32-C3 and WixNet WixFi360 if I only want wifi. I am prioritizing bluetooth, so I am going to try my luck with the ESP32-C3 first.
![image](https://fabricate.hackclub-assets.com/2a1e754e85ec2d6e70d027fe8f8fb9cc7d6e8544452bc43e76cace4a59f2a515/image-png/image.png)

**Total time spent: 1h 30m**

# September 10, 2026: Found a ESP32-C3 Microcontroller

So the ESP32-C3 is a separate chip. I could switch my entire devboard to that, but I want to keep what I have., not overhaul my entire project. I landed on the ESP32-C3-WROOM-02-N4. It's based on the ESP32-C3 (Shocker) and is basically a microcontroller SoC in a chip package. Anyways, I will be using that so I can focas on the communicating between both chips, and not making another devboard. This lets me learn multi chip coding, without making 2 chips.

link to chip: https://jlcpcb.com/partdetail/3281215-ESP32_C3_WROOM_02N4/C2934560

I won't be using the gpio on this. I have enough already. I will be instead focusing on interchip communication, so i can use the Esp32's wifi/bluetooth LT in the future. Also, I am limited heavily by the speed of the UART I will be using to communicate with it.I agree I should have used the ESP from the get go, but it's a learning process.
![image](https://fabricate.hackclub-assets.com/b82e63ad8922af5e91fe7ea4ccca321e0d8045dcf00874f56e5b636cdb2ce067/image-png/image.png)

**Total time spent: 1h 36m**
