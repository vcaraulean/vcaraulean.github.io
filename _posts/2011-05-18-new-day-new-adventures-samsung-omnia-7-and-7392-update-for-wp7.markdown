---
comments: true
date: 2011-05-18 10:35:23
layout: post
title: New day, new adventures - Samsung Omnia 7 and 7392 update for WP7
categories: [windows-phone]
---

I’ve been glad to see that Microsoft started again rolling out the 7392 update for Omnia 7. We, geeks, are loving updates. It gets us more stability and new features. And bugs, sometimes...

The 7392 is a minor one, it should fix some issues with security certificates. It’s not critical, but you’ll have to install it in order to get all next updates.

When MS [announced that he resumes rolling this update](http://windowsteamblog.com/windows_phone/b/windowsphone/archive/2011/05/17/updates-resume-to-omnia-7-phones.aspx) they also mentioned that “a small number of people ... might have trouble ...”. By Murphy's law I’ve landed in that small group that’s having troubles. How it was:

  * Zune said it **has an update** for my phone. 
  * Zune is **trying** to install update but can’t move past “rebooting” stage. No probs, Microsoft [warned about it an provided a workaround](http://support.microsoft.com/kb/2547687). 
  * Installed the tool from Samsung, following instructions how to enter the phone in Download Mode. 
  * The phone is **not entering in Download Mode**, so Samsung’s tool cannot update the firmware so then Zune can install 7392 update. 
  * The only option to be tried is, as suggested by the tool and MS’s guide, is to **contact Samsung** Service Center. 
 
After googling around I’ve found that Omnia 7 phones with bootloader version 4.10.1.9 cannot enter in Download Mode by pressing VOLUMEUP + CAMERA keys. It’s disabled in this version. 

Looking further I’ve found that I have 2 options to get this update (and, consequently all next updates that will be rolled from now on):

  * **Contact Samsung’s Service Center**. The recommended way. This probably will mean that you have to send your phone somewhere in order to get it serviced. Will call them to see how much time will take that, but looks it’s not an option for me. 
  * **Tweak the hardware to enter Download Mode**. The phones that can’t enter this mode by with known key combination can be forced to to it using a special (modified) PC connection cable or a Mini-USB dongle attached to the phone. Search Ebay for “Samsung Galaxy S Download Mode USB jig”, you’ll get the idea. 8$ + shipping. Plugging in this dongle will set the phone to Download Mode and you’ll be able to run [Samsung’s utility](http://support.microsoft.com/kb/2547687) to update the phone. Or you’ll be able to flash a new [ROM for device](http://www.samfirmware.com). See [Omnia 7 wiki on xds-developers](http://forum.xda-developers.com/wiki/index.php?title=Samsung_Omnia_7) for details about phone and flashing. 

Second options isn’t a recommended way to do it. You can kill your phone. Do it at your own risk. But most probably that’s what I’ll be trying to do, because I don’t have any willing to send my phone to a service center even for few days let alone for few weeks. 

### Update 01/06/2011:

As I’ve received today the USB jig that I’ve mentioned earlier, this is the follow up.

Switched phone off, plugged in the jig & wait for Download Mode to appear on the screen, pulled it out then connected to the PC. Samsung’s tool detected the phone and offered to update my firmware. Less than a minute later it congratulates with a successful update and rebooted the phone. 

After reboot Zune started and offered to install the 7.0.7392 update. Accepted.

It passed successfully the glorious “rebooting” stage and after doing the backup my phone **was successfully updated** to latest Windows Phone OS. Now waiting for Mango...
