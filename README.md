make-mb41-osx
=============
###### OS X 10.10/10.11 installer creator for MacBook4,1 (Mac-F22788A9)

###What doesn't work?
* Brightness control — I found the PCI register that controls brightness, but running `setpci` on it seems to do nothing. Still looking into it, hopefully I can solve this.
* X3100 QECI acceleration — OS X is still very usable without this, because the X3100 framebuffer still works. So you will still be able to use external displays, and use all 144MB of VRAM, etc.
* Display wake from sleep — Sleeping and waking works, but the display will remain off. This is likely due to QECI acceleration not being present.
* [10.11 only] USB is complete broken on OS X 10.11. It's an issue with AppleUSBEHCIPCI, looking into fixing this. Will take some time.

###What works?
* Well... everything else. :P **This includes FaceTime/iMessage/parts of Continuity (SMS Forwarding, iPhone phone call forwarding), too!**

###To-do
* Add feature for user to disable beamsync (`beamsyncoff`)
* Get HDA working via DummyHDA instead of replacing AppleHDA
* Fix brightness control
* Add feature to make Recovery HD usable for MacBook4,1 (important for 10.11!)
* [10.11] Fix USB (AppleUSBEHCIPCI)
* [10.10] Add script that disables Mission Control animations

###How is the performance?
I currently am running OS X Yosemite 10.10.5 as a daily driver on my MacBook4,1. Performance is pretty good, actually. Performing the "Post-installation steps" at the end of the tutorial is a good idea.

I'm rather satisfied with it, actually. Video playback is pretty decent with MPlayerX, and YouTube videos also works fine. If you're using Chrome, install [h264ify](https://chrome.google.com/webstore/detail/h264ify/aleakchihdccplidncghkekgioiakgal?hl=en-US) for better YouTube performance. Or use Safari. ┐(￣ー￣)┌

Simple OpenGL games like VVVVVV and Super Hexagon run fine, but I won't expect too much from this.

###Okay, so how do I use this?
Before we begin, I want to make it clear that this tool should only be used by those who are comfortable working with the Terminal, and have some degree of experience with OS X.

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
1. `cd /Volumes/Install OS X Yosemite/` (or `El Capitan`)
1. `sh postinst.sh`
1. Enter in the **disk name** of the disk in which you installed OS X to.
1. Quit Terminal, and allow your MacBook4,1 to reboot into your freshly-installed OS X.

#### Post-installation steps (recommended, but optional)
1. Set up OS X as you would normally.
1. To improve performance, I recommend that you disable Beam Sync. A number of tools can do this, including "BeamOff" and "BeamSyncDropper2". Use Google, I guess.
1. To make Mission Control usable (and prevent it from crashing WindowServer), I recommend that you disable its animations. You can do that by pasting this into Terminal: `defaults write com.apple.dock expose-animation-duration -float 0; killall Dock`
1. Enable "Reduce Transparency" in Settings -> Accessibility.

### Troubleshooting

* **I didn't make it in time to run postinst.sh and my MacBook4,1 rebooted into a gray screen with a cross on it! D:**
    * That's okay, just boot from the USB installer again, and perform those steps again. Your OS X install has not been harmed :P
* **I updated OS X and now all I see is a gray screen with a cross!**
    * Boot from the USB installer again, and perform the steps pertaining to `postinst.sh` again.
* **Booting from Recovery HD only results in a gray screen with a cross!**
    * `make-mb41-osx` does not yet make Recovery HD work. I'm working on that.
* **My 10.11 El Capitan installer isn't booting! All I see is a gray screen with a cross!**
    * USB support is currently broken for the MacBook4,1 under 10.11 at this time. Therefore, the installer cannot boot from the USB, as it doesn't even detect the USB to begin with. I'm working on this, the issue lies in `AppleUSBEHCIPCI.kext`.
* **I updated to 10.11 El Capitan and now my USB ports don't work!**
    * USB doesn't work under 10.11 yet. Reinstall 10.10 for now.
