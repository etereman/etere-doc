[Return to main page](../README.md)

# Playlist Mode description
This page describes playlist mode. Playlist mode allow you to change current effect automatically according to `show.txt` file.

## Default Interaction
For example there are 4 effects on SD card.
- `S1.txt`
- `S3.txt`
- `S4_rain.txt`
- `S12.txt`    

And special file
- `show.txt` with following text inside.
```
1, 0:00 //Run effect S1 at start
2, 0:10 //Run effect S2 at 10th second
3, 0:25 
0, 0:30 
4, 0:31 
12, 0:35:541 //Turn S12 at 35.541
1, 40100 //Turn S1 at 40100 ms = 40s 100ms
0, 0:50
3, 1:05 //Keep effect S3
```

When controller loaded you are in `blackout`.
- If you set this mode as default in configuration file `play.default=Playlist;`, you start in Playlist Mode.
- If you keep defaults, you should press button for 5 seconds and release on Cyan color.

If you press any button mode will come to active state, effects will change according to `show.txt` file and keep last state at end.

So `blackout` \> Click \> 
```
1, 0:00         | Set S1 at start (0th second)
2, 0:10         | S2 can't be found on sd Card -> ignoring
3, 0:25         | Set S3 after 25 seconds from click.
0, 0:30         | Set blackout at 30th second from click.
4, 0:31         | Set S4 at second 31
12, 0:35:541    | Set S12 at 35.541
1, 40100        | Set S1 at 41.1 after click
0, 0:50         | Set blackout at second 50
3, 1:05         | Set S3 at 1:05, keeping effect
```
after 1:05 mode will come to inactive state keeping effect `S3.txt`. If you click button again controller will start playlist from the start.
>**Note:** If you want to set suit black at end, simply place 0 and time to set blackout at end. For this example => `0, 1:05`

>**Note:** After coming to working state suit will blink in cyan indicating start of work. Any other clicks will be ignored.  

If you perform `reset action` (Keep button pressed for 1.5 sec and releasing) you will automatically switched to `blackout state` and work will stop there.

>**Note:** Click again to switch back to working state after reset.

## File creation

As you might noticed, file contains commentaries after lines. You can add your own commentary after symbol "`/`" inside file to make file more readable for you.  

First number is `effect id`, `0` is reserved for blackout,  
You can use any separator you want, and perform formatting you prefer more. Like this:
```
1 - 0:00 / girls at stage
  4 with rain at 0:05 / introduction
  3 0:10
12, 0:15:500 / boys at stage
...
0 -> 1:15 /show end, suits off
```

>**Note:** symbol "`:`" is reserved for time formatting, don't use it for regular formatting, also you can have text commentary even without symbol "`/`" but make sure not using numbers inside such comments.

>**Note:** make sure you end commentary line with "`Enter`" key.

You can use time in different formats
- 11:43:430 `/ or 11 minutes 43 seconds 430 milliseconds`
  >**Note:** 11:43:43 => 11 minutes 43 seconds 43 milliseconds
- 12:21 `/ or 12 minutes 21 seconds`
- 40204 `/ or 40 seconds 204 milliseconds`

>**Note:** make sure you are not having spaces between ":" and numbers

Time calculated from switching to active state. Each line describes switch to next effect.  
If line time lesser than previous it will be set immediately
```
12, 1:00 /Will show first frame of effect S12
11, 0:55 /And will be switched to effect S11
```
If effect with selected `effect id` not exist, changing wouldn't be performed.


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
  On state change, controller sends `1` multiple times each 7ms for duration `radio.sendEach=ms;`.
  >**Note:** So controller in this mode, you can use external radio module (like 20 button console or different) to change state.
  
  >**! Important:** You can have different `show.txt` on suits, but make sure files end at same time, because if playlist end, controller will send reset action to all suits ending their work, also keeping last effect not possible when interacting with Group Mode.

## Availability
This mode available only if file `show.txt` can be located on SD card and contains at least 1 line.  
You can find it at second position in que (or lesser if other mods unavailable). Suit will glow in Purple.

>**Note:** You can check that suit correctly understood your playlist at the end of `log.txt` file.

## Technical details
Mode prefix is `[ P ]` inside log file.  
If `play.autostart=1;` and mode is set as default, then on load controller will automatically start changing effects.
>**Note:** Rarely customers ask us to help them configure show for their performance, we use this mode + set this mode as default in configuration file.
  
You can download example file and put it on your SD card:  
[show.txt example file](https://raw.githubusercontent.com/isadora-6th/etere-doc/master/modes/show.txt) (file from example)

>**Note:**: Right click on page and press `Save As...`  

Your playlist file can have any number of lines you actually need.  
Time over 60 minutes can be set by simply increasing number of minutes, so 95:40 -> 1 hour 35 minutes 40 seconds.  
It's actually works if you set seconds count over 60, so - 0:65 -> 1 minute 5 seconds.

#

Commentaries starting with '`/`' are actually ending with '`\n`' or '`LF`', Enter key in Windows set '`CR+LF`', on Mac OS only '`LF`' so both platforms work.

>**Note:** On older versions , reset action through radio just stopped switching effects, not setting `blackout state`.