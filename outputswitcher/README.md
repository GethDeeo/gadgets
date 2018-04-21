# OutputSwitcher

This is basically just a small wrapper script around two fantastic pieces of software, namely
[12noon](https://12noon.com/)'s **Display Changer II** (for changing display configuration) and
[NirSoft](https://nirsoft.net/)'s **NirCmd** (for changing sound playback device).  
It has some use to Windows users, it's written in JScript utilizing Windows Script Host
([WSH](https://en.wikipedia.org/wiki/Windows_Script_Host)), which should be available and enabled by default.

I wrote it to help myself out in a very specific situation: I have a desktop PC and an HDTV hooked to it via HDMI,
but the two devices reside in different rooms. If I wanted to play some game on the HDTV (wireless controllers, yay),
I had to switch video *and* audio output before starting it. To further complicate it, I've found out that many titles
only play nice if the OS is configured in single display mode.

## Setup

1. Install [Display Changer II](https://12noon.com/?page_id=641).
2. Install [NirCmd](https://nirsoft.net/utils/nircmd.html).
3. Change your display configuration you'll want to switch to, and save it with **Display Changer II** like this:  
`dc2.exe -create=my_configuration.xml`  
That will create a configuration file, you'll need that in the next step.  
Of course, you can switch back afterwards :smirk:
4. Place a copy of **OutputSwitcher.wsf** somewhere, and adjust the resources in it.  
They're commented and have sample values.  
For files you can leave off the path part, iff the file is on the `PATH`.  
(Optionally, you can localize some other message resources, the ones that starts with `message`, just don't touch the
`#placeholders#` in them.)
5. (Per game) If you have a game whose executable is `game1.exe`, you have to have a shortcut for it. Modify that
shortcut's "Target" like this:  
From: `X:\path\to\game1.exe`  
To: `"%SystemRoot%\System32\wscript.exe" "Y:\path\to\OutputSwitcher.wsf" "X:\path\to\game1.exe"`

You can now use this modified shortcut to launch the game, and before it actually launches, you're be asked whether you
want to switch outputs or not. If you decide to switch, after exiting back to desktop, video output will be restored
along with the previously configured audio playback device.

## Some more thoughts

* The game has to have single "command" to launch (understandable by the OS shell) without any further command line
arguments. If it's not the case, I recommend to wrap them in some sort of `.cmd` batch script using `START /WAIT ...`
and use that script in the shortcut's Target instead of the game's executable.
* You have to adjust "normal" audio playback device in the script too, because **NirCmd** can't query the current one.
* If for some reason you use more than one alternate configuration, you can just create another copy of the script,
configure it, and use the copy in the shortcut's Target. I know it may be cumbersome, but it's enough for me :wink:
