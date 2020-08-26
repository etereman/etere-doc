[Return to main page](../README.md)

# Auto Mode description
This page describes auto mode. Auto mode allow you to change current effect automatically skipping blackout state.

## Default Interaction
For example there are 4 effects on SD card.
- `S1.txt`
- `S3.txt`
- `S4_rain.txt`
- `S12.txt`    

And special file
- `auto.txt` with number 15000 inside.

When controller loaded you are in `blackout`.
- If you set this mode as default in configuration file `play.default=Auto;`, you start in Auto Mode.
- If you keep defaults you should press button for 5 seconds and release on Purple color.

If you press any button mode will come to active state, effects will change each 15000 milliseconds (or 15 seconds), and skip blackout state.  
So `blackout` \> `S1.txt` \>15s\> `S3.txt` \>15s\> `S4_rain.txt` \>15s\> `S12.txt` \>15s\> `S1.txt` \>15s\> ...

>**Note:** After coming to working state suit will blink in purple indicating start of work. Any other clicks will be ignored.  

If you perform `reset action` (Keep button pressed for 1.5 sec and releasing) you will automatically switched to `blackout state` and work will stop there.

>**Note:** Click again to switch back to working state after reset.

So `S4_rain.txt` \> `blackout` when `reset action` done.

## Radio Work
- Radio Receive Mode:  
  Any number except reserved will cause mode active state.  
  So receiving for example `5` will switch to active state. Send `reset (255)` to switch back to inactive state.   
  `255` reserved for reset action.  
  `0` reserved for ignore.  
    >**Note:** So controller in this mode, you can use external radio module (like 20 button console or different) to change state.
- Radio Send Mode:  
  Sends current state:
  - `1` for active.
  - `255` for inactive.  
  
  Send performed each `radio.sendEach=ms;` where `ms` is 30 by default.

- Radio Group Mode:  
  On state change multiple times sends `1` each 7ms for duration `radio.sendEach=ms;`.
  >**Note:** So controller in this mode, you can use external radio module (like 20 button console or different) to change state.

## Availability
This mode available only if file `auto.txt` can be located on SD card and contains number.  
You can find it at third position in que (or lesser if other mods unavailable). Suit will glow in Purple.

## Technical details
Mode prefix is `[ A ]` inside log file.  
If `play.autostart=1;` and mode is set as default, then on load controller will automatically start changing effects.
>**Note:** We use this mode for our standalone installations for photo zones, we simply use power switch and work started, no need to go and click buttons.
  
You can download example file and put it on your SD card:  
[auto.txt example file](https://raw.githubusercontent.com/isadora-6th/etere-doc/master/modes/auto.txt) (12 seconds)

>**Note**: Right click on page and press `Save As...`  

or you can create file yourself and copy following line to file
```
12000 /Time in msec after that effect changes, 12000 = 12 sec, 5000 = 5 sec, 65000 = 65 sec...
```

You can add extra text if you want, only requirement is to have number at first place. So following line is valid too.
```
65000
```