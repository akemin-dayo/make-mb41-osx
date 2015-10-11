make-mb41-osx
=============
###### OS X 10.10/10.11 installer creator for MacBook4,1 (Mac-F22788A9)

###What doesn't work?
* Brightness control — I found the PCI register that controls brightness, but I haven't been able to actually set the brightness yet. Still looking into it.
* X3100 QECI acceleration — **OS X is still very usable without this, because the X3100 framebuffer still works.** So you will still be able to use change resolutions, external displays, and use all 144MB of VRAM, etc.
* Display wake from sleep — Sleeping and waking works, but graphics completely fail to re-initialise. Probably something to do with missing QECI acceleration.
* [10.11 only] USB is completely broken on OS X 10.11. It's an issue with AppleUSBEHCIPCI, I have a few ideas I'm going to test regarding this.

###Okay, what works?
* Well... everything else. :P **This includes FaceTime/iMessage/parts of Continuity (SMS Forwarding, iPhone phone call forwarding), too!**

###To-do list
* Bundle some sort of Beam Sync disabler tool (or write my own to be included)
* Write a script that allows the user to disable Mission Control animations without using the `defaults` command in the Terminal.
* Integrate `postinst.sh` into the installation process itself.
* Write an rc.shutdown script that deletes `/System/Library/CoreServices/PlatformSupport.plist` if found. (And if found, replace AppleHDA again.)
* Get HDA working via DummyHDA instead of outright replacing AppleHDA (difficult, because I believe the AppleHDA binary needs to be modified)
* Fix brightness control
* Add feature to make Recovery HD usable for MacBook4,1 (very important for 10.11 due to SIPCSP)
* [10.11 only] Fix USB (AppleUSBEHCIPCI)

###How does it perform?
I actually am currently running OS X Yosemite 10.10.5 as a daily driver on my MacBook4,1. Performance is actually very good. I do recommend that you perform the "Post-installation steps" at the end of my tutorial below for best results.

But anyway, I'm rather satisfied with the performance. Video playback is pretty decent with MPlayerX, and web videos (such as YouTube) decode fine.

I will say that YouTube videos perform better under Safari than in Chrome. If you're like me and have an undying love for Chrome, install [h264ify](https://chrome.google.com/webstore/detail/h264ify/aleakchihdccplidncghkekgioiakgal) for better YouTube performance.

...Or you can just use Safari to watch YouTube videos. ┐(￣ー￣)┌

Simple OpenGL games like VVVVVV and Super Hexagon run fine, but really, don't expect anything too complex from this. (I mean, even if it *did* have QECI acceleration, it *is* still a GMA X3100...)

On a slightly entertaining note, Parallels Desktop 11 (running Windows 10) performs decently, too. I actually did not expect this :P

###Okay, so how do I use this?

#### Preparing the installer USB
1. Make sure you have your desired OS X installer downloaded from the App Store and placed in `/Applications/`.
1. Open Terminal
1. `git clone https://github.com/angelXwind/make-mb41-osx.git`
1. `cd make-mb41-osx`
1. `sudo ./make-mb41-osx 10.10` (or `10.11`, but because USB is broken, there is no point in creating a 10.11 installer with this tool, for now)
1. Enter in the disk number of your USB in the prompt that appears. **WARNING: ALL DATA WILL BE LOST ON THE DISK NUMBER YOU ENTER!**
1. Sit back, and wait. **Note: During the process, Finder may open the installer USB — this does NOT mean it is complete.**
1. After it completes, eject the USB and plug into your MacBook4,1.

#### Installing OS X on your MacBook4,1
1. Turn on your MacBook4,1 holding down Option (Alt)
1. Boot from "OS X Base System" (orange icon)
1. Install OS X.
1. Wait for a bit.
1. **When the process is nearly complete, start paying attention.**
1. **When the installer starts counting down until a reboot, quickly go to the menubar and go to Utilities -> Terminal**
1. `cd "/Volumes/Install OS X Yosemite/"` (or `El Capitan`)
1. `sh postinst.sh`
1. Enter in the **disk name** of the disk in which you installed OS X to.
1. Quit Terminal, and allow your MacBook4,1 to reboot into your freshly-installed OS X.

#### Post-installation steps (recommended, but optional)
1. Set up OS X as you would normally.
1. To improve performance, I recommend that you disable Beam Sync. A tool written by JasF called BeamOff ([[precompiled binary download]](https://www.sendspace.com/file/sm9sf7), [[source on GitHub]](https://github.com/JasF/beamoff)) can do this for you — just add it as a Login Item and you're golden.
1. To make Mission Control actually usable (and to prevent it from potentially crashing WindowServer), I recommend that you disable its animations. You can do that by pasting this into Terminal: `defaults write com.apple.dock expose-animation-duration -float 0; killall Dock`
1. Enable "Reduce Transparency" in Settings -> Accessibility.

### Troubleshooting/FAQs

* **What's a MacBook4,1?**
    * All of these polycarbonate White/Black MacBooks: http://www.everymac.com/ultimate-mac-lookup/?search_keywords=MacBook4,1
* **I didn't make it in time to run `postinst.sh` and my MacBook4,1 rebooted into a gray screen with a cross on it! D:**
    * That's okay, just boot from the USB installer again, and perform those steps again. Your OS X install has not been harmed :P
* **I updated OS X and now all I see is a gray screen with a cross!**
    * Boot from the USB installer again, and perform the steps pertaining to `postinst.sh` again.
* **Booting from Recovery HD only results in a gray screen with a cross!**
    * `make-mb41-osx` does not yet make Recovery HD work. I'm working on that.
* **My 10.11 El Capitan installer isn't booting! All I see is a gray screen with a cross!**
    * USB support is currently broken for the MacBook4,1 under 10.11 at this time. Therefore, the installer cannot boot from the USB, as it doesn't even detect the USB to begin with. I'm working on this, the issue lies in `AppleUSBEHCIPCI.kext`.
* **I updated to 10.11 El Capitan and now my USB ports don't work!**
    * USB doesn't work under 10.11 yet. Reinstall 10.10 for now.